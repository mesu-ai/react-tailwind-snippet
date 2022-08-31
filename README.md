# React Tailwind 
# ----------------------------------------------

## Tailwind Modal

### Modal Body
import { useRef, useState } from 'react';
import ModalHeading from '../atoms/ModalHeading';

const Modal = ({ title = '', children }) => {
	const [open, setOpen] = useState(false);

	const targetRef = useRef(null);
	console.log(targetRef);

	// useEffect(() => {
	// 	const handleShowModal = (event) => {
	// 		if (targetRef && !targetRef?.current?.contains(event.target)) {
	// 			setOpen(false);
	// 		}
	// 	};
	// 	if (open) window.addEventListener('click', handleShowModal);
	// 	return () => window.removeEventListener('click', handleShowModal);
	// }, [open]);

	return (
		<div className="">
			<button ref={targetRef} onClick={() => setOpen(true)} type="button">
				open modal
			</button>

			{/* modal content */}

			{open && (
				<div className="flex justify-center items-center fixed inset-0 bg-gray-600 bg-opacity-50 overflow-y-auto h-screen w-full">
					<div className="relative  mx-auto py-5 border w-1/2 shadow-lg  bg-white">
						{/* Modal header */}
						<ModalHeading title={title} setOpen={setOpen} />
						<hr className="mt-4 border-t border-mercury" />
						{/* Modal body Start */}
						<div className="mt-10 text-center">{children}</div>
					</div>
				</div>
			)}
		</div>
	);
};

export default Modal;


### Modal Heading
import ClosebarIcon from '../../assets/svgs/ClosebarIcon';

const ModalHeading = ({ title = '', setOpen = Boolean }) => {
	return (
		<div className="flex justify-between px-7">
			<p className="font-bold text-2xl text-darkblack">{title}</p>
			<button type="button" onClick={() => setOpen(false)}>
				<ClosebarIcon />
			</button>
		</div>
	);
};

export default ModalHeading;


### Reusable Radio Button

const [selectItem, setSelectItem] = useState(packages[0]);
	const selectPackage = (pack) => {
		setSelectItem(pack);
		// console.log(pack, selectItem);
	};

	//radio component call
	<RadioButton
							key={item.name}
							selectPackage={selectPackage}
							selectItem={selectItem}
							packages={packages}
							item={item}
							index={index}
						/>

const RadioButton = ({ packages, item, index, selectPackage, selectItem }) => {
	return (
		<button type="button" onClick={() => selectPackage(item)}>
			<label htmlFor={item.name}>
				<input
					type="radio"
					name={item}
					className="block mx-auto cursor-pointer"
					// defaultChecked={index === 0}
					checked={packages[index] === selectItem}
					onChange={() => selectPackage(item)}
				/>
				<span>{item.name}</span>
				<p className="text-lg text-center">{item.value}</p>
			</label>
		</button>
	);
};

export default RadioButton;


### File Upload and preview

/* eslint-disable no-nested-ternary */
/* eslint-disable no-unused-expressions */
import { useState } from 'react';
import ClosebarIcon from '../../assets/svgs/ClosebarIcon';
import UploadFileIcon from '../../assets/svgs/UploadFileIcon';
import Card from '../../components/atoms/Card';
import SectionDivider from '../../components/atoms/SectionDivider';

const imagePreview = { img1: '', img2: '', img3: '' };

export const ImageUpload = () => {
	const [selectedFile, setSelectedFile] = useState([]);
	const [preview, setPreview] = useState('');

	const onSelectFile = (e) => {
		if (!e.target.files || e.target.files.length === 0) {
			setSelectedFile(undefined);
			return;
		}

		const { name } = e.target;
		const file = e.target.files[0];

		const files = { ...selectedFile };
		files[name] = file;
		setSelectedFile(files);

		setPreview([...preview, name]);

		const objectUrl = URL.createObjectURL(file);
		if (name === 'img1') {
			imagePreview.img1 = objectUrl;
		} else if (name === 'img2') {
			imagePreview.img2 = objectUrl;
		} else if (name === 'img3') {
			imagePreview.img3 = objectUrl;
		}
	};

	const handleRemoveFile = (file) => {
		if (file) {
			file === 'img1'
				? delete selectedFile.img1
				: file === 'img2'
				? delete selectedFile.img2
				: delete selectedFile.img2;
		}

		const remainPreview = preview.filter((item) => item !== file);
		setPreview(remainPreview);
	};

	return (
		<div>
			<Card className="py-7 px-5">
				<p className="text-darkblack text-lg font-semibold mb-5">Upload Image (up to 3)</p>
				<SectionDivider column="sm:grid-cols-4">
					{['img1', 'img2', 'img3'].map((item, index) => (
						<Card key={item} className="h-48 rounded-lg relative">
							{preview.indexOf(item) === -1 ? (
								<label
									htmlFor={item}
									className="flex justify-center items-center border-mercury  cursor-pointer  w-full h-full"
								>
									<UploadFileIcon />
									<input
										multiple
										id={item}
										type="file"
										className="hidden"
										name={item}
										onChange={(e) => onSelectFile(e)}
									/>
								</label>
							) : (
								<>
									{/* {index === 0 && (
										<img className="w-full h-full rounded-lg" src={imagePreview.img1} alt="Upload_img_1" />
									)}
									{index === 1 && (
										<img className="w-full h-full rounded-lg" src={imagePreview.img2} alt="Upload_img_2" />
									)}
									{index === 2 && (
										<img className="w-full h-full rounded-lg" src={imagePreview.img3} alt="Upload_img_3" />
									)} */}
									<img
										className="w-full h-full rounded-lg"
										src={index === 0 ? imagePreview.img1 : index === 1 ? imagePreview.img2 : imagePreview.img3}
										alt="Preview_img"
									/>
									<button
										className="absolute top-4 right-4 rounded-full px-0.5 py-1.5"
										style={{ backgroundColor: 'rgba(0, 0, 0, 0.16)' }}
										type="button"
										onClick={() => handleRemoveFile(item)}
									>
										<ClosebarIcon className="h-4" color="white" />
									</button>
								</>
							)}
						</Card>
					))}
				</SectionDivider>
			</Card>
		</div>
	);
};

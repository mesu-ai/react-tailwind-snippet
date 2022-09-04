# React Tailwind 

### Tailwind Modal

### Modal Body

```
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

```

### Modal Heading

```
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

```

### Reusable Radio Button

```
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

```

### File Upload and preview

```
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

```


## Multiline Stepper 

```
import { useState } from 'react';

const Stepper = () => {
	const [step, setStep] = useState(0);

	const nextStep = () => {
		if (step <= 2) {
			setStep((prevActiveStep) => prevActiveStep + 1);
		}
	};
	const prevStep = () => {
		if (step >= 1) {
			setStep((prevActiveStep) => prevActiveStep - 1);
		}
	};

	

	console.log(step);

	return (
		<div className="p-5">
			<div className="mx-4 p-4">
				<div className="flex  items-center text-center">
					<div className={`flex items-center  relative `}>
						<div className="rounded-full transition duration-500 ease-in-out h-12 w-12 py-3 bg-action/[0.2]">1st</div>
						<div className="absolute top-0 -ml-10 rtl:-mr-10 text-center mt-16 w-32 text-xs font-medium uppercase text-darkblack">
							Overview
						</div>
					</div>
					<div
						className={`flex-auto border-t-2 transition duration-500 ease-in-out border-lightgray ${
							step >= 1 ? 'border-action' : 'border-lightgray'
						}`}
					/>
					<div className="flex items-center text-white relative">
						<div
							className={`rounded-full transition duration-500 ease-in-out h-12 w-12 py-3  ${
								step >= 1 ? 'bg-action/[0.2]' : 'bg-ghostwhite'
							}`}
						>
							2nd
						</div>
						<div
							className={`absolute top-0 -ml-10 rtl:-mr-10 text-center mt-16 w-32 text-xs font-medium uppercase ${
								step >= 1 ? 'text-darkblack' : 'text-lightgray'
							}`}
						>
							Pricing
						</div>
					</div>
					<div
						className={`flex-auto border-t-2 transition duration-500 ease-in-out border-lightgray ${
							step >= 2 ? 'border-action' : 'border-lightgray'
						}`}
					/>
					<div className="flex items-center text-white relative">
						<div
							className={`rounded-full transition duration-500 ease-in-out h-12 w-12 py-3  ${
								step >= 2 ? 'bg-action/[0.2]' : 'bg-ghostwhite'
							}`}
						>
							3nd
						</div>
						<div
							className={`absolute top-0 -ml-10 rtl:-mr-10 text-center mt-16 w-32 text-xs font-medium uppercase ${
								step >= 2 ? 'text-darkblack' : 'text-lightgray'
							}`}
						>
							Description
						</div>
					</div>
					<div
						className={`flex-auto border-t-2 transition duration-500 ease-in-out border-lightgray ${
							step >= 3 ? 'border-action' : 'border-lightgray'
						}`}
					/>
					<div className="flex items-center text-white relative">
						<div
							className={`rounded-full transition duration-500 ease-in-out h-12 w-12 py-3  ${
								step >= 3 ? 'bg-action/[0.2]' : 'bg-ghostwhite'
							}`}
						>
							4th
						</div>
						<div
							className={`absolute top-0 -ml-10 rtl:-mr-10 text-center mt-16 w-32 text-xs font-medium uppercase ${
								step >= 3 ? 'text-darkblack' : 'text-lightgray'
							}`}
						>
							Gallery
						</div>
					</div>
				</div>
			</div>

			{step === 0 && (
				<div className="mt-20">
					<p>step 0 component</p>
				</div>
			)}

			{step === 1 && (
				<div className="mt-20">
					<p>step 1 component</p>
				</div>
			)}

			<div className="flex p-2 mt-4">
				<button
					onClick={prevStep}
					type="button"
					className="text-base hover:scale-110 focus:outline-none flex justify-center px-4 py-2 rounded font-bold cursor-pointer 
            hover:bg-gray-200  
            bg-gray-100 
            text-gray-700 
            border duration-200 ease-in-out 
            border-gray-600 transition"
				>
					Previous
				</button>
				<div className="flex-auto flex flex-row-reverse">
					<button
						onClick={nextStep}
						type="button"
						className="text-base  ml-2  hover:scale-110 focus:outline-none flex justify-center px-4 py-2 rounded font-bold cursor-pointer 
            hover:bg-teal-600  
            bg-teal-600 
            text-teal-100 
            border duration-200 ease-in-out 
            border-teal-600 transition"
					>
						Next
					</button>
					<button
						type="button"
						className="text-base hover:scale-110 focus:outline-none flex justify-center px-4 py-2 rounded font-bold cursor-pointer 
            hover:bg-teal-200  
            bg-teal-100 
            text-teal-700 
            border duration-200 ease-in-out 
            border-teal-600 transition"
					>
						Skip
					</button>
				</div>
			</div>
		</div>
	);
};

export default Stepper;

```

### Input Field Tag Name

```

import React, { useState } from 'react';

import Card from '../atoms/Card';

const Overview = () => {
	
	const [tagNames, setTagNames] = useState([]);


	const addTags = (tags) => {
		
		tags
			.slice()
			.reverse()
			.forEach((tag) => {
				// console.log(tag);
				setTagNames([...tagNames, tag]);
			});
	};
	const handleKeyPress = (e) => {
		const tags = [];
		if (e.key === 'Enter') {
			e.target.value.split(',').forEach((tag) => {
				tags.push(tag);
			});

			addTags(tags);

			e.target.value = '';
		}
	};
	console.log(tagNames);

	const handleRemoveTag = (tag) => {
		const remainTags = tagNames.filter((item) => item !== tag);
		setTagNames(remainTags);
	};

	return (
		<div>
				<p className="mb-2">Tag</p>
				<div className="p-5 flex flex-wrap items-start justify-start  rounded-2xl border border-mercury min-h-[112px] w-full  text-darkblack">
					{tagNames &&
						tagNames.map((tag) => (
							<div key={tag}>
								<span className="inline mx-2 overflow-clip">{tag}</span>
								<button type="button" onClick={() => handleRemoveFile(tag)}>
									<ClosebarIcon className="h-4" />
								</button>
							</div>
						))}
					<input type="text" onKeyUp={(e) => handleKeyPress(e)} className="outline-none inline " />
				</div>
		</div>
	);
};

export default Overview;

```
### Add File and show name

```
import ClosebarIcon from '../../assets/svgs/ClosebarIcon';
import FileIcon from '../../assets/svgs/FileIcon';

const AddFile = () => {
	const [ addFiles, setAddFiles] =useState([]);

	const handleFile = (file) => {
		const newFile = [...addFiles];
		newFile.push(file);
		setAddFiles(newFile);
	};
	const handleRemoveFile = (file) => {
		const remainFiles = addFiles.filter((item) => item !== file);
		setAddFiles(remainFiles);
	};
	return (
		<div className="flex flex-row-reverse">
			<label htmlFor="file-upload" className="border-mercury inline-block p-2 cursor-pointer mx-4">
				<FileIcon />
				<input id="file-upload" type="file" className="hidden" onChange={(e) => handleFile(e.target.files[0])} />
			</label>
			<div className="flex">
				{addFiles.map((file) => (
					<div key={file.name} className="flex border-mercury px-3 py-2 shadow w-fit rounded-lg mx-2">
						<p className="px-2 text-slategray">{file?.name}</p>

						<button type="button" onClick={() => handleRemoveFile(file)}>
							<ClosebarIcon className="h-4" />
						</button>
					</div>
				))}
			</div>
		</div>
	);
};

export default AddFile;

```
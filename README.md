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

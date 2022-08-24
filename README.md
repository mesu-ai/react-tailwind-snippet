### Tailwind Modal

# Modal Body
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


# Modal Heading
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
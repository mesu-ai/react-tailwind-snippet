# React Tailwind 



### Navbar with mobile responsiveness

#### close icon
```ruby

const ClosebarIcon=({ height = 24, width = 24, className = '', color = 'currentColor' })=> {
    return (
        <svg
            width={height}
            height={width}
            viewBox={`0 0 ${height} ${width}`}
            className={className}
            fill="none"
            stroke={` ${color}`}
            strokeWidth="2"
            xmlns="http://www.w3.org/2000/svg"
        >
            <path strokeLinecap="round" strokeLinejoin="round" d="M6 18L18 6M6 6l12 12" />
        </svg>
    );
}


export default ClosebarIcon;



```

#### menu icon
```ruby

const MenubarIcon=({ height = 24, width = 24, className = '' })=> {
    return (
        <svg
            width={height}
            height={width}
            viewBox={`0 0 ${height} ${width}`}
            className={className}
            fill="none"
            stroke="currentColor"
            strokeWidth="2"
            xmlns="http://www.w3.org/2000/svg"
        >
            <path strokeLinecap="round" strokeLinejoin="round" d="M4 6h16M4 12h16M4 18h16" />
        </svg>
    );
}



export default MenubarIcon;

```

#### primary navbar

```ruby
import { useState } from 'react';
import ClosebarIcon from '../../assets/ClosebarIcon';
import MenubarIcon from '../../assets/MenubarIcon';
import MobileScreen from './MobileScreen';

const PrimaryNavbar=()=> {
    const [open, setOpen] = useState(false);

    return (
        <nav className={`w-full   z-50  'mt-0' : 'mt-8'} `}>
            <div className=" mx-4 lg:mx-10 px-5 py-3 dark:bg-darkBg bg-white rounded-full shadow-lg ">
                <div className="relative container flex items-center justify-between h-16">
                    <div className="flex items-center justify-center sm:justify-start gap-6">
                        <div className="col-span-1 block">Logo</div>

                        <div className="hidden md:block sm:mx-6 md:mx-auto">
                            <div className="flex flex-row justify-center items-center space-x-5 list-none">
                                menu
                            </div>
                        </div>
                    </div>

                    {/* absolute inset-y-0 right-0 rtl:right-auto rtl:left-0  sm:static sm:inset-auto */}
                    <div className="  flex items-center   space-x-3 md:space-x-6 rtl:space-x-reverse">
                        language button
                        {/* <Dropdown title={<GlobIcon />} left="ltr:-left-72 rtl:-right-32">
                            <div className="px-5">
                                <LangChange />
                            </div>
                        </Dropdown> */}
                        <div className=" flex items-center md:hidden">
                            <button
                                type="button"
                                onClick={() => setOpen(!open)}
                                className="inline-flex items-center justify-center p-2 rounded-md text-gray-400 hover:text-white hover:bg-gray-700 focus:outline-none focus:ring-2 focus:ring-inset focus:ring-white"
                            >
                                <span className="sr-only">Open main menu</span>
                                {open ? (
                                    <ClosebarIcon className="h-6 w-6" aria-hidden="true" />
                                ) : (
                                    <MenubarIcon className="h-6 w-6" aria-hidden="true" />
                                )}
                            </button>
                        </div>
                    </div>
                </div>
                {open && <MobileScreen setOpen={setOpen} />}
            </div>
        </nav>
    );
}

export default PrimaryNavbar;

```
#### mobile navbar

```ruby
const MobileScreen=()=> {
    return (
        <div className="relative">
            <div
                className={`py-10 block xl:hidden absolute  top-4 left-0 right-0 bottom-0  bg-white/95 dark:bg-darkBg/95 z-50 rounded-xl min-h-[600px] overflow-y-scroll `}
            >
                <div className=" space-y-2">
                    <ul className="items-start gap-6 flex flex-col px-10 ">menu</ul>
                    {/* <div className="px-10 pt-6">
                        <DropdownButton title="More" dropDownList={dropDownList} arrow />
                    </div> */}
                    {/* {!token && (
                        <div className="flex sm:hidden  justify-center items-center gap-4  pt-20">
                            <LoginModal />
                            <RegisterModal />
                        </div>
                    )} */}
                </div>
            </div>
        </div>
    );
}

export default MobileScreen;


```


### Mega menu/secondary navbar/ dropdown
```ruby
/* eslint-disable jsx-a11y/no-static-element-interactions */

import NavigationDropdown from '../../components/molecules/NavigationDropdown';
import { secondaryNavItems } from '../../routes/headerRoutes';

const SecondaryNav = () => {
	return (
		<div className="hidden lg:block   py-3" style={{ backgroundColor: 'rgba(245, 246, 250, 0.8)' }}>
			<div className="container mx-auto  px-2 xl:px-10">
				<div className="relative">
					<div className="flex justify-between text-sm ">
						{secondaryNavItems.map((item, index, items) => (
							<NavigationDropdown key={item.id} menuItem={item} items={items} />
						))}
					</div>
				</div>
			</div>
		</div>
	);
};

export default SecondaryNav;

```
```ruby
/* eslint-disable no-nested-ternary */
import { useEffect, useRef, useState } from 'react';
import { useTranslation } from 'react-i18next';

const NavigationDropdown = ({ items = 0, menuItem = 0 }) => {
	// eslint-disable-next-line no-unused-vars
	const [show, setShow] = useState(false);
	const targetRef = useRef(null);
	const { t } = useTranslation();
	const handleToggle = () => setShow((prevState) => !prevState);

	useEffect(() => {
		const handleShowprofileMenu = (event) => {
			if (targetRef && !targetRef?.current?.contains(event.target)) {
				setShow(false);
			}
		};
		if (show) window.addEventListener('click', handleShowprofileMenu);

		return () => window.removeEventListener('click', handleShowprofileMenu);
	}, [show]);

	return (
		<div className="relative inline-block text-left">
			<div>
				<button
					className="text-slategray flex justify-between"
					ref={targetRef}
					type="button"
					id="menu-button"
					aria-expanded="true"
					aria-haspopup="true"
					onClick={handleToggle}
				>
					{t(`${menuItem?.categoryName}`)}
				</button>
			</div>

			{show && (
				<div
					className={`origin-top-right absolute mt-5 w-96 rounded-xl px-4 pb-8 pt-4 z-50  bg-white ring-1 ring-black ring-opacity-5 focus:outline-none ${
						items[0] === menuItem
							? '-right-40 rtl:right-0'
							: items[items.length - 1] === menuItem
							? 'right-0 rtl:-right-80'
							: '-right-28'
					}`}
					style={{
						boxShadow: '0px 15px 37px 2px rgba(0, 0, 0, 0.16)',
					}}
					role="menu"
					aria-orientation="vertical"
					aria-labelledby="menu-button"
					tabIndex="-1"
				>
					<p className="text-start font-semibold text-darkblue text-lg py-3 px-5">{menuItem?.categoryName}</p>
					<hr className="border border-mercury mb-3" />
					<div className="py-1" role="none">
						{menuItem?.subItems.map((item) => (
							<a
								key={item.id}
								href="#"
								className="block rounded-xl hover:bg-action hover:bg-opacity-10 z-10 space-y-2 px-5  py-2"
								role="menuitem"
								tabIndex="-1"
								id="menu-item-0"
							>
								<p className="font-medium text-[#353945] text-start">{item?.name}</p>
							</a>
						))}
					</div>
					<p className="text-action font-semibold px-4 mt-3">Others &gt;</p>
				</div>
			)}
		</div>
	);
};

export default NavigationDropdown;

```
### React countdown

```ruby
import Countdown, { zeroPad } from 'react-countdown';
import Card from '../atoms/Card';

const renderer = ({ days, hours, minutes, completed }) => {
	if (completed) {
		// Render a complete state
		return <p>Time Out</p>;
	}

	const updatedDays = zeroPad(days);
	const updatedHours = zeroPad(hours);
	const updatedMinutes = zeroPad(minutes);
	// Render a countdown

	return (
		<div className="flex justify-center gap-3 xm:gap-5 sm:gap-8">
			<div>
				<Card className="text-action p-4 sm:p-7 font-bold  text-xl xm:text-3xl sm:text-5xl">
					<span>{String(updatedDays)[0]}</span>
					<span className="border-l-2 border-mercury mx-3 sm:mx-5" />
					<span>{String(updatedDays)[1]}</span>
				</Card>
				<p className="text-center mt-[10px] text-sm xm:text-base  text-darkblack font-medium">DAYS</p>
			</div>

			<div>
				<Card className="text-action p-4 sm:p-7 font-bold  text-xl xm:text-3xl sm:text-5xl">
					<span>{String(updatedHours)[0]}</span>
					<span className="border-l-2 border-mercury mx-3 sm:mx-5" />
					<span>{String(updatedHours)[1]}</span>
				</Card>
				<p className="text-center mt-[10px] text-sm xm:text-base  text-darkblack font-medium">HOURS</p>
			</div>

			<div>
				<Card className="text-action p-4 sm:p-7 font-bold  text-xl xm:text-3xl sm:text-5xl">
					<span>{String(updatedMinutes)[0]}</span>
					<span className="border-l-2 border-mercury mx-3 sm:mx-5" />
					<span>{String(updatedMinutes)[1]}</span>
				</Card>
				<p className="text-center mt-[10px] text-sm xm:text-base  text-darkblack font-medium">MINUTES</p>
			</div>
		</div>
	);
};

const TimeCountdown = () => {
	return (
		<div>
			<Countdown date={new Date('2022-10-30').getTime()} renderer={renderer} />
		</div>
	);
};

export default TimeCountdown;


```

### Tailwind Modal

### Modal Body

```ruby
import { useRef, useState } from 'react';
import ModalHeading from '../atoms/ModalHeading';

const Modal = ({ title = '', children }) => {
	const [open, setOpen] = useState(false);

	const targetRef = useRef(null);
	console.log(targetRef);

	  useEffect(() => {
	        const body = document.body;
	        const originalStyle = window.getComputedStyle(body).overflowY;
	       
	        const scrollbarWidth = window.innerWidth - body.clientWidth;
	        console.log({originalStyle,scrollbarWidth})
	    
	        if (open) {
	            body.style.overflowY = 'hidden';
	            body.style.marginRight = `${scrollbarWidth}px`;
	            
	        } else {
	            body.style.overflowY = originalStyle;
	            body.style.marginRight = '0';
	        }
	    
	        return () => {
	            body.style.overflowY = originalStyle;
	            body.style.marginRight = '0';
	        };
	    
	    }, [open]);

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

```ruby
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

```ruby
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


### Multiline Stepper 

```ruby
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

```ruby

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

### Accrodion

```
import { useState } from 'react';
import ArrowDownSlate from '../../assets/svgs/ArrowDownSlate';
import ArrowUpSlate from '../../assets/svgs/ArrowUpSlate';

const Accrodion = ({
	className = '',
	title = 'Accrodion Title',
	titleStyle = 'text-darkblack',
	isOpen = false,
	children,
}) => {
	const [open, setOpen] = useState(isOpen);
	return (
		<div className={`border-2 border-mercury rounded-lg ${className}`}>
			<button
				type="button"
				onClick={() => setOpen((prevState) => !prevState)}
				className={`w-full px-6 py-4 flex justify-between ${open ? 'border-b-2 border-mercury' : ''}`}
			>
				<p className={`text-base font-bold  ${titleStyle}`}>{title}</p>
				{open ? <ArrowUpSlate /> : <ArrowDownSlate />}
			</button>
			{open && <div>{children}</div>}
		</div>
	);
};

export default Accrodion;

for map of accrodian and open nth element isOpen={array.length===index}

<Accrodion className="mb-4" title="Order declined" titleStyle="text-danger uppercase" isOpen={false}>
			content here							
</Accrodion>
```

### Formic text field
```
import { Field, useField } from 'formik';
import { useTranslation } from 'react-i18next';

const InputText = ({
	label = '',
	lebelStyle = '',
	errorStyle = 'text-center',
	as = '',
	type = '',
	name = '',
	placeholder = '',
	className = '',
	...props
}) => {
	const { t } = useTranslation();
	const [field, meta, helpers] = useField({ name });

	return (
		<>
			<label className={`mb-2  ${lebelStyle}`} htmlFor={name}>
				{t(`${label}`)}
				<Field
					as={as}
					type={type}
					className={`w-full outline-none inline mx-2  ${className}`}
					name={name}
					placeholder={t(`${placeholder}`)}
					{...props}
				/>
			</label>

			{meta.touched && meta.error ? (
				<div className={`error pt-2 text-red-600 ${errorStyle}`}>{t(meta.error)}</div>
			) : null}
		</>
	);
};

export default InputText;

```
### Formic radio button
```

import { Field, useField } from 'formik';
import { useState } from 'react';
import { useTranslation } from 'react-i18next';

const InputRadio = ({ items = [], name = '', errorStyle = 'hidden', children = {} }) => {
	const [customTime, setCustomTime] = useState();
	const [field, meta] = useField({ name });
	const { t } = useTranslation();

	// console.log({ field });

	return (
		<>
			{items.map((item) => (
				<label
					key={item.id}
					className={`cursor-pointer flex justify-center border-r-2 border-mercury ${
						field?.value === item?.value ? 'font-bold text-darkblue' : ''
					}`}
					htmlFor={item.id}
				>
					<Field {...field} type="radio" className="hidden" id={item.id} name={name} value={item.value} />
					{item.value} {parseInt(item.value, 10) === 1 ? 'day' : 'days'}
				</label>
			))}

			{children}
			{meta.touched && meta.error && (
				<div className={`error pt-2 text-red-600 text-start ${errorStyle}`}>{t(meta.error)}</div>
			)}
		</>
	);
};

export default InputRadio;

```
### Formic select field
```
/* eslint-disable react/no-array-index-key */
/* eslint-disable jsx-a11y/label-has-associated-control */
/* eslint-disable no-unused-vars */
import { Field, useField } from 'formik';
import { useEffect } from 'react';
import { useTranslation } from 'react-i18next';

const InputSelect = ({
	label = '',
	className = 'mt-2',
	labelClass = 'text-lg text-darkblack',
	optionColor = 'text-black',
	name = '',
	opts = {},
	items = [],
	defaultLabel = 'Select',
	dispatch,
}) => {
	const { t, i18n } = useTranslation();
	const [field, meta, helpers] = useField({ name });

	useEffect(() => {
		if (dispatch) {
			dispatch({ type: 'INBOX_SORTING', payload: field?.value });
		}
	}, [dispatch, field?.value]);

	return (
		<>
			<label className={` mb-2 font-semibold  ${labelClass}`}>{t(`${label}`)}</label>
			<Field
				as="select"
				name={name}
				className={` w-full flex justify-between outline-none border border-mercury p-3 rounded-xl ${className}`}
			>
				{defaultLabel && <option>{t(`${defaultLabel}`)}</option>}
				{items &&
					items.map((item) => (
						<option key={item.id} value={item[opts.value] || item.value} className={`${optionColor}`}>
							{i18n.language === 'ar' ? item?.name_ar : item?.name}
						</option>
					))}
			</Field>
			{meta.touched && meta.error ? <div className="error pt-2 text-red-600 text-start">{t(meta.error)}</div> : null}
		</>
	);
};

// {
// 	items.map((item) => (
// 		<option key={item.id} value={item[opts.value] || item.value} className="text-black">
// 			{i18n.language === 'ar' ? item.name_ar : item.name}
// 		</option>
// 	));
// }

export default InputSelect;

```
### Formic Password
```
import { Field, useField } from 'formik';

import { useTranslation } from 'react-i18next';
import { boolean } from 'yup';
import { EyeIcon, HideIcon } from '../../assets/svgs';

const InputFeild = ({
	icon = {},
	showicon = false,
	placeholder = '',
	name = '',
	type = '',
	setShowPassword = boolean,
	showPassword = [],
}) => {
	// eslint-disable-next-line no-unused-vars
	const [field, meta, helpers] = useField({ name });
	const { t } = useTranslation();

	return (
		<div className="mb-5">
			<div className="flex rounded-lg py-3 px-3 w-full bg-ghostwhite">
				<div className="flex justify-center items-center">{icon}</div>

				<Field
					className="outline-0 mx-2 w-full text-sm font-medium bg-ghostwhite placeholder:text-slategray"
					name={name}
					placeholder={t(`${placeholder}`)}
					type={type}
				/>

				{showicon &&
					(showPassword ? (
						<button onClick={() => setShowPassword(!showPassword)} type="button">
							<EyeIcon className="w-4 h-5" />
						</button>
					) : (
						<button onClick={() => setShowPassword(!showPassword)} type="button">
							<HideIcon className="w-4 h-5" />
						</button>
					))}
			</div>

			{meta.touched && meta.error ? <div className="error text-start text-red-600">{t(meta.error)}</div> : null}
		</div>
	);
};

export default InputFeild;

```
### Rating with Formic

```
import { Field, useField } from 'formik';
import { useTranslation } from 'react-i18next';
import StarIcon from '../../assets/svgs/StarIcon';

const Rating = ({ name = '', className = 'h-6 w-6' }) => {
	const { t } = useTranslation();
	const [field, meta, helpers] = useField({ name });

	return (
		<div>
			<label htmlFor={name}>
				<Field name={name} type="number" id={name} className="hidden" />

				{[...Array(5)].map((_, index) => {
					index += 1;
					return (
						<button
							key={index}
							type="button"
							onClick={() => helpers.setValue(index)}
							onMouseEnter={() => helpers.setValue(index)}
							onMouseLeave={() => helpers.setValue(index)}
						>
							<StarIcon className={`${className}`} color={`${index <= field.value ? '#F2B556' : '#E6E8EC'}`} />
						</button>
					);
				})}
			</label>

			{meta.touched && meta.error ? <div className="error pt-2 text-red-600 text-start">{t(meta.error)}</div> : null}
		</div>
	);
};

export default Rating;

```
### Show rating
```
/* eslint-disable no-param-reassign */
/* eslint-disable react/no-array-index-key */

import StarIcon from '../../assets/svgs/StarIcon';

const ShowRatings = ({ ratings = 5, reviews = 100,  }) => {
	return (
		<div className="flex justify-center items-center gap-1">
		
					{[...new Array(5)].map((_, index) => {
						index += 1;
						return <StarIcon key={index} color={`${index <= Math.ceil(ratings) ? '#F2B556' : '#E6E8EC'}`} />;
					})}
					<p className="ms-1 text-yellow font-semibold">
						<span>{ratings}</span>
						<span> ({reviews})</span>
					</p>
		</div>
	);
};

export default ShowRatings;

```
### Attechment download
```
import axios from 'axios';
import download from 'js-file-download';
import icon from '../../assets/images/AttachmentLarge.png';
import DownloadIconAction from '../../assets/svgs/DownloadIconAction';

const AttachmentDownload = () => {
	const fileDownload = (url) => {
		const fileName = 'Filename';
		axios
			.get(url, {
				responseType: 'blob',
			})
			.then((res) => {
				download(res.data, fileName);
			});
	};
	return (
		<div className="border-2 border-mercury rounded-xl w-40">
			<div className="grid place-content-center">
				<img className="p-10" src={icon} alt="attachment icon" />
			</div>
			<div className="border-t-2 border-mercury py-2 px-3 flex  justify-between">
				<p className="text-xs font-medium text-darkblue">File Name</p>
				<button onClick={(url) => fileDownload(url)} type="button">
					<DownloadIconAction />
				</button>
			</div>
		</div>
	);
};

export default AttachmentDownload;

```

### video player
```
import { useState } from 'react';
import PlayButtonIcon from '../../assets/svgs/PlayButtonIcon';

const VideoPlayer = ({ srcvideo = '' }) => {
	const [play, setPlay] = useState(false);
	const myVideo = document.getElementById('videoplayId');
	const playPause = () => {
		if (srcvideo && myVideo.paused) {
			myVideo.play();
			setPlay(true);
		} else if (srcvideo) {
			myVideo.pause();
			setPlay(false);
		}
	};

	return (
		<div className="text-center relative group">
			<video id="videoplayId" width="500" height="300" src={srcvideo}>
				{/* <source src={srcvideo} type="video/mp4" /> */}
				<track src="" kind="captions" srcLang="en" label="english_captions" />
			</video>
			<button
				className={`absolute inset-0 ${play ? 'hidden' : 'block'} group-hover:block`}
				type="button"
				onClick={playPause}
			>
				<PlayButtonIcon className="mx-auto" />
			</button>
		</div>
	);
};

export default VideoPlayer;

```

### Table

```
//main component
const Table = ({ children, className = 'w-full', tableContainer = ' ' }) => {
	return (
		// rounded-2xl
		<div className={`overflow-x-auto relative rounded-2xl ${tableContainer}`}>
			<table className={`text-sm text-left text-gray-500 ${className}`}>{children}</table>
		</div>
	);
};

export default Table;

```

```
//heading title
import { useTranslation } from 'react-i18next';

const TableHeadData = ({ title, className = '' }) => {
	const { t } = useTranslation();
	return (
		<th scope="col" className={`py-3 px-6 ${className}`}>
			{t(`${title}`)}
		</th>
	);
};

export default TableHeadData;


```

```
//row data
/* eslint-disable react/jsx-props-no-spreading */
const TableData = ({ children, className = '', colSpan = '' }) => {
	return (
		<td className={`px-6 py-8 ${className}`} colSpan={colSpan}>
			{children}
		</td>
	);
	// py-4 px-6 pt-5 pb-8
};

export default TableData;



```

```
<Table tableContainer="border border-mercury">
					<thead className=" text-gray-700  bg-smoke  rounded-t-lg">
						<tr className="rounded-t-lg">
							{accountStatementsLevel &&
								accountStatementsLevel.map((title) => <TableHeadData key={title} title={title} />)}
						</tr>
					</thead>
					<tbody>
						{accountStatements &&
							accountStatements.map((item) => (
								<tr key={item.id} className=" bg-white border-b text font-medium">
									<TableData className="text-darkblue font-bold">{item?.id}</TableData>
									<TableData className="text-darkblue font-bold">{item?.sellerName}</TableData>
									<TableData>{item?.item}</TableData>
									<TableData className="text-darkblue font-bold">{item?.quantity}</TableData>
									<TableData>{item?.duration}</TableData>
									<TableData className="text-darkblue font-bold">{item?.deliveryDate}</TableData>
									<TableData className="text-darkblue font-bold">{item?.totalPrice}</TableData>
									<TableData className="flex items-start">
										<Button
											className={`mt-0.5 py-3 px-5 w-32 ${
												item.paymentStatus === 'Completed'
													? 'bg-thingreen text-[#35C27F]'
													: 'bg-thinyellow text-[#F2B556]'
											}`}
											title={item?.paymentStatus}
										/>
									</TableData>
								</tr>
							))}
					</tbody>
				</Table>
```

### Rating component

```ruby
import StarIcon from '@/assets/svgs/StarIcon';
import React from 'react';

const Rating = ({rating=4}) => {

    return (
        <li className='flex'>
            {[...new Array(5)].map((_, index) => {
                index += 1;
                return <StarIcon className='h-3 w-3 sm:w-4 sm:h-4' key={index} color={index <= Math.ceil(rating) ? '#F47920' : '#E6E8EC'} />
            })}
        </li>
    );
};

export default Rating;

```

### Product Image Zoom (show side)

```ruby


import React, { useEffect, useRef } from 'react';

const ImageZoom = ({ imageSrc, imageWidth, imageHeight}) => {
  const lensRef = useRef(null);
  const resultRef = useRef(null);
  const imgRef = useRef(null);

  const imageZoom = (img, result, lens, e) => {
    const { width: imgWidth, height: imgHeight } = img.getBoundingClientRect();

    const lensWidth = imgWidth / 3; // Adjust the lens size as needed
    const lensHeight = imgHeight / 3;

    const cx = img.width / lensWidth;
    const cy = img.height / lensHeight;

    result.style.width = `${imgWidth}px`; // Set the result size to match the image size
    result.style.height = `${imgHeight}px`;

    lens.style.width = `${lensWidth}px`;
    lens.style.height = `${lensHeight}px`;

    const imgRect = img.getBoundingClientRect();
    let x = e.clientX - imgRect.left - lensWidth / 2;
    let y = e.clientY - imgRect.top - lensHeight / 2;

    // Ensure the lens stays within the image boundaries
    x = Math.max(0, Math.min(imgWidth - lensWidth, x));
    y = Math.max(0, Math.min(imgHeight - lensHeight, y));

    lens.style.left = `${x}px`;
    lens.style.top = `${y}px`;

    const bgPosX = -(x * cx);
    const bgPosY = -(y * cy);

    result.style.backgroundImage = `url(${img.src})`;
    result.style.backgroundSize = `${imgWidth * cx}px ${imgHeight * cy}px`;
    result.style.backgroundPosition = `${bgPosX}px ${bgPosY}px`;
  };

  useEffect(() => {
    const handleMouseMove = (e) => {
      const img = imgRef.current;
      const lens = lensRef.current;
      const result = resultRef.current;

      if (img && lens && result) {
        const imgRect = img.getBoundingClientRect();
        if (
          e.clientX > imgRect.left &&
          e.clientX < imgRect.right &&
          e.clientY > imgRect.top &&
          e.clientY < imgRect.bottom
        ) {
          lens.style.display = 'block';
          result.style.display = 'block';
          imageZoom(img, result, lens, e);
        } else {
          lens.style.display = 'none';
          result.style.display = 'none';
        }
      }
    };

    document.addEventListener('mousemove', handleMouseMove);
    return () => {
      document.removeEventListener('mousemove', handleMouseMove);
    };
  }, [imageSrc]);



  return (
    <div>
      <div className="relative">
        <img
          src={imageSrc}
          alt="Zoomable"
          className="border rounded-md mx-auto"
          ref={imgRef}
          width={imageWidth}
          height={imageHeight}
        />
        <div
          id="lens"
          ref={lensRef}
          className="absolute bg-gray-200 bg-opacity-40 border border-gray-400 hidden"
          style={{ width: imageWidth / 3, height: imageHeight / 3 }}
        ></div>

      </div>
      <div
        id="result"
        ref={resultRef}
        className="z-10 absolute top-1/2 -translate-y-1/2 right-0 bg-no-repeat border border-gray-400 hidden"
        style={{ width: imageWidth, height: imageHeight }}
      ></div>
    </div>
  );
};

export default ImageZoom;


```



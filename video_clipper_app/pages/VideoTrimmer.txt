// "use client"; 
// import { useEffect, useRef, useState } from "react";
// import Nouislider from 'nouislider-react';
// import 'nouislider/distribute/nouislider.css';
// import { createFFmpeg, fetchFile } from "@ffmpeg/ffmpeg";

// const ffmpeg = createFFmpeg({ log: true });

// export default function VideoTrimmer({file, fileUrl}) {
//     const videoRef = useRef(null);
//     const sliderRef = useRef(null);
//     const [videoFile, setVideoFile] = useState(null);
//     const [videoURL, setVideoURL] = useState(null);
//     const [trimmedVideo, setTrimmedVideo] = useState(null);
//     const [videoDuration, setVideoDuration] = useState(100);

//     useEffect(() => {
//         if (sliderRef.current) {
//             noUiSlider.create(sliderRef.current, {
//                 start: [0, 100],
//                 connect: true,
//                 range: { min: 0, max: 100 },
//                 step: 1,
//                 tooltips: true,
//                 format: {
//                     to: (value) => parseFloat(value).toFixed(1),
//                     from: (value) => parseFloat(value),
//                 },
//             });

//             sliderRef.current.noUiSlider.on("update", (values) => {
//                 if (videoRef.current) {
//                     videoRef.current.currentTime = parseFloat(values[0]);
//                 }
//             });
//         }
//     }, []);

//     // const handleFileChange = (event) => {
//     //     const file = event.target.files[0];
//     //     if (file) {
//     //         setVideoFile(file);
//     //         const url = URL.createObjectURL(file);
//     //         setVideoURL(url);
//     //     }
//     // };
//     setVideoFile(file);
//     setVideoURL(fileUrl);

//     const handleLoadedMetadata = () => {
//         if (videoRef.current) {
//             const duration = videoRef.current.duration;
//             setVideoDuration(duration);
//             sliderRef.current.noUiSlider.updateOptions({
//                 range: { min: 0, max: duration },
//                 start: [0, duration],
//             });
//         }
//     };

//     const handleTrimVideo = async () => {
//         if (!ffmpeg.isLoaded()) await ffmpeg.load();
//         if (!videoFile) return;

//         const fileName = "input.mp4";
//         ffmpeg.FS("writeFile", fileName, await fetchFile(videoFile));

//         const [start, end] = sliderRef.current.noUiSlider.get().map(parseFloat);
//         const duration = end - start;

//         await ffmpeg.run("-i", fileName, "-ss", start.toString(), "-t", duration.toString(), "-c", "copy", "output.mp4");

//         const data = ffmpeg.FS("readFile", "output.mp4");
//         const trimmedURL = URL.createObjectURL(new Blob([data.buffer], { type: "video/mp4" }));
//         setTrimmedVideo(trimmedURL);
//     };

//     return (
//         <div>
//             {videoURL && (
//                 <>
//                     <video ref={videoRef} src={videoURL} controls onLoadedMetadata={handleLoadedMetadata} />
//                     <div ref={sliderRef} style={{ margin: "20px 0" }}></div>
//                     <button onClick={handleTrimVideo}>Trim Video</button>
//                 </>
//             )}
//             {trimmedVideo && (
//                 <div>
//                     <h3>Trimmed Video:</h3>
//                     <video src={trimmedVideo} controls />
//                 </div>
//             )}
//         </div>
//     );
// }

# Dataset description

Introduction
The proposed technique of laser speckle imaging with sub-pixel correlation analysis [1] allowed the identification of changes in the sterile (or inhibition) zone diameter (around the antibiotic) and to obtain the resulting inhibition zone diameter earlier than the disk diffusion method, which is recommended by the European Committee on Antimicrobial susceptibility testing (EUCAST) [2-4]. Deep learning algorithms make it possible to automatically, with high accuracy, classify the appearance of inhibition zones, separating them from zones with active bacteria. Results could improve mathematical models of changes in the diameter of the inhibition zone around the disc containing the antimicrobial agent, thereby speeding up and facilitating epidemiological analysis.

Data Description
During the experiment performance, an image file (speckle-images) is saved at constant time intervals. This time interval may differ for different experiments, but it does not change in one experiment. We perform image acquisition at 10, 20 or 30 seconds for bacteria and 1 or 4 sec for fungi, corresponding to the Nyquist–Shannon sampling theorem [5-6].
The file format is "png" or "jpg". The size of each file varies depending on the experiment. Currently, this is about 10-20 MB, and in pixels 2748 x 3840 pixels. The experiment duration 10-50 hours. Thus, after the experiment implementation, several thousand such files ( ~ 15 MB each ) are obtained.
Since most of the processing is done in the time domain (although a number of procedures are also performed in the spatial domain), it is necessary to accumulate the files in time. Since 1 file is 10 MB or more, then thousands of files are GB-s and it is impossible to save them through Matlab and work with them.

Data organization
1) The entire field of the experiment (Petri dish) was divided into small sections of NxN pixels. The algorithms are performed between consecutive images of NxN pixels from the beginning to the end of the experiment.
That is, instead of many files in which 1 image is stored, we get files where 3-D arrays are stored, but covering small areas of the field.
2) After saving the files (step 1), each of them is processed, turning N x N x time sampling into 1 x time sampling signals.
Accordingly, if the array dimension was: (K*N)x(K*N)x(time points) sampling, then now they become (K)x(K) x(time points) sampling. It turns out 2 main signals of this dimension: a) Averaging over spatial coordinates ( x,y ), b) Shift of the correlation peak (and its more accurate location using interpolation ) over spatial coordinates ( x,y ). 
These three signals and the time vector are stored as binary files. The files are significantly smaller in volume than in step 1. Further work and processing is done with these files containing "signals" [1].
A comparison of the method with others can be found in the articles [7-8].

Data provided:
To see what the received signals look like, analyze them for yourself, make images, etc., they are placed in folders in the binary mat files format, and below it is described what variables are contained inside each file. The files relate to: fungi, bacteria, the sterile (inhibition) zone around the antibiotic, and from the antibiotic.
For bacteria, inhibition zone, and antibiotic, data were taken from an E. coli experiment conducted on 29.11.2022.  For fungi, data were taken from an Aspergillus niger experiment conducted on 07.02.2024. Since the behaviour and development of fungi is faster than that of bacteria, pay attention to the difference in sampling frequency [6].
Data inside files:
1) Fs - sampling frequency (milliHz)          
2) sigX: [4x4x(Ns-1) ] - 4x4 = 16 signals (accumulation of shifts along the X axis), each signal taken from an area of 10x10 original pixels  
3) sigY: [4x4x(Ns-1) ] - 4x4 = 16 signals (accumulation of shifts along the Y axis), each signal taken from an area of 10x10 original pixels   
4) t - time vector in seconds [1xNs]
5) Stat - stricture with fields
5.1) Avg: [4×4×Ns] - 4x4 = 16 signals (each signal is the average of a section of 10x10 original pixels) 
5.2) Med: [4×4×Ns] - 4x4 = 16 signals (each signal is the median of a section of 10x10 original pixels) 
5.3) StD: [4×4×Ns] - 4x4 = 16 signals (each signal is the standard deviation of a section of 10x10 original pixels)

Classification between inhibition and active growth microorganisms zone

A detailed description can be found in our article [9].
Data provided:
For classification purposes (between inhibition zones and zones with active bacterial growth), files of the test set are included. It refers to the entire experimental field (thousands of signals) divided into short time-intervals. For a training set, need to take several experiments similar in volume and structure (about 10), but they are not included here.
Test set:
The experiment was divided into 1-hour intervals from the beginning to the end of the experiment. If the experiment was carried out for 17 hours, then 17 mat files. Each file contains the following variables:  
1) "Labels_All" - 260x360, 2D matrix where the real class of each point in space for a given time window is indicated.  Whereas “1” is an antibiotic, “2” is a zone of inhibition, “3” is a zone of active bacterial growth. For image presentation only.
2) "Labels" - 1x93600 vector, obtained from the matrix "Labels_All" - 260x360 = 93600. For use in processing (neural network, etc.)
3) MatAv - 260x360, 2D matrix “Spatial Averaging Algorithm” signal, matrix at the end of the hour interval (that is, only one, the last point of the hour). For image presentation only.
4) ImgAv - 93600x46 Matrix of “Spatial Averaging Algorithm”, 260x360 = 93600 and 46 - number of samps per hour, (after the temporal smoothing algorithm). That is, not the last point, but all points for the current hour. For use in processing (neural network, etc.)
5) MatSig - 260x360, 2D matrix “Subpixel Correlation Algorithm” signal, matrix at the end of the hour interval (that is, only one, the last point of the hour). For image presentation only.
6) ImgSig - 93600x46 Matrix of “Subpixel Correlation Algorithm”, 260x360 = 93600 and 46 - the number of samps per hour (after the temporal smoothing algorithm). That is, not the last point, but all points for the current hour. For use in processing (neural network, etc.)
7-8) PositionX, PositionY - 1x93600 vectors connecting the size of signals 93600 with points in the XY plane (260x360).  
9-10) "t2" - is the time vector (in hours) from the beginning to the end of the experiment. For example 17 hours. Since each file takes a short time interval (1 hour), the "tind" variable indicates the indexes that need to be taken from the "t2" vector for this time window.
A detailed description can be found in our article [9] , also mentioned in [3].
Additional general materials about the method and its application [10-15]

References
1.	Balmages, I., Liepins, J., Zolins, S., Bliznuks, D., Lihacova, I., & Lihachev, A. Laser speckle imaging for early detection of microbial colony forming units. Biomed. Opt. Express 12(3), 1609–1620 (2021).
2.	The European Committee on Antimicrobial Susceptibility Testing - EUCAST 2022, Clinical breakpoints - breakpoints and guidance, available online: https://www.eucast.org/clinical_breakpoints/
3.	I. Balmages, J. Liepins, S. Zolins, D. Bliznuks, R. Broks, I. Lihacova, A. Lihachev  "Tools for classification of growing/non growing bacterial colonies using laser speckle imaging". Frontiers in Microbiology, 14, 1279667, (2023), DOI :10.3389/fmicb.2023.1279667
4.	I. Balmages, A. Reinis, S. Kistkins, D. Bliznuks, E. V. Plorina, A. Lihachev, and I. Lihacova, "Laser speckle imaging for visualization of hidden effects for early detection of antibacterial susceptibility in disc diffusion tests", Frontiers in Microbiology, 14, 1221134, (2023), DOI: 10.3389/fmicb.2023.1221134
5.	I. Balmages, D. Bliznuks, J. Liepins, S. Zolins, and A. Lihachev, “Laser speckle time-series correlation analysis for bacteria activity detection,” Proc. SPIE 11359, 113591D (2020).
6.	I. Balmages, A. Reinis, S. Kistkins, J. Liepins, M. Pogorielov, V. Korniienko,  K. Diedkova,  D. Bliznuks, A. Lihachev, I. Lihacova "Determination of operating parameters of fungal growth signals analyzed by laser speckle contrast imaging", Proc. SPIE 13006, Biomedical Spectroscopy, Microscopy, and Imaging III, (2024), DOI: 10.1117/12.3016906
7.	I. Balmages, A. Reinis, S. Kistkins, D. Bliznuks, A. Lihachev, I. Lihacova "Comparison of algorithms for monitoring the behavior of microorganisms based on remote laser speckle method", Proc. SPIE 13196, Artificial Intelligence and Image and Signal Processing for Remote Sensing XXX, 1319611 (2024); https://doi.org/10.1117/12.3032500
8.	I. Balmages, K. Smite, D. Bliznuks, A. Reinis, S. Kistkins, A. Lihachev, I. Lihacova, "Correlation analysis techniques of laser speckle images and rationale for its use to assess the growth and behavior of fungi and bacteria", (2025), (in press)
9.	I. Balmages, D. Bliznuksa, I. Polakab, A. Lihachevc, I. Lihacovac, "Classification of Microbial Activity and Inhibition Zones Using Neural Network Analysis of Laser Speckle Images", (2025) (in press)
10.	I. Balmages, J. Liepins, E. T. Auzins, D. Bliznuks, E. Baranovics, I. Lihacova, and A. Lihachev, "Use of the speckle imaging sub-pixel correlation analysis in revealing a mechanism of microbial colony growth," Sci. Rep. 13(1), 2613, (2023), DOI: 10.1038/s41598-023-29809-0
11.	I. Balmages, D. Bliznuks, A. Reinis, S. Kistkins, E. V. Plorina, A. Lihachev, and I. Lihacova, "Laser speckle imaging-assisted disk diffusion test for early estimation of sterile zone radius", Proc. SPIE 12628, Diffuse Optical Spectroscopy and Imaging IX, 126281Y, (2023); DOI: 10.1117/12.2670618
12.	I. Lihacova, I. Balmages, A. Reinis, S. Kistkins, D. Bliznuks, E. V. Plorina, A. Lihachev, "Dynamic laser speckle imaging for fast evaluation of the antibacterial susceptibility by the disc diffusion method", 19th Nordic-Baltic Conference (NBC) on Biomedical Engineering and Medical Physics (2023); DOI: 10.1007/978-3-031-37132-5_39
13.	A. Lihachev, I. Balmages, J. Liepins, I. Lihacova, D. Bliznuks, "Dynamic laser speckle imaging for estimation of microbial colony growth in a noisy environment", Proc. SPIE 12572, Optical Sensors 2023; 1257220 (2023); DOI: 10.1117/12.2675953
14.	I. Balmages, J. Liepins, E. T. Auzins, A. Zile, D. Bliznuks, I. Lihacova, A. Lihachev "Evaluation of microbial colony growth parameters by laser speckle imaging", Proc. SPIE 12144, Biomedical Spectroscopy, Microscopy, and Imaging II, 121440B (2022); DOI: 10.1117/12.2621152
15.	I. Balmages, J. Liepins, D. Bliznuks, S. Zolins, I. Lihacova, A. Lihachev, "Laser speckle imaging reveals bacterial activity within colony", Proc. SPIE 11920, Diffuse Optical Spectroscopy and Imaging VIII; 1192024 (2021), DOI: 10.1117/12.2615444


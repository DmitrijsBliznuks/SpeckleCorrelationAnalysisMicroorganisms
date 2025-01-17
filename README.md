# SpeckleCorrelationSet1

The experiment data was divided into 1-hour intervals from the beginning to the end of the experiment. If the experiment was carried out for 17 hours, then 17 mat files. Each file contains the following variables:  


1) "Labels_All" - 260x360, 2D matrix where the real class of each point in space for a given time window is indicated.  Whereas “1” is an antibiotic, “2” is a zone of inhibition, “3” is a zone of active bacterial growth. For image presentation only.
2) "Labels" - 1x93600 vector, obtained from the matrix "Labels_All" - 260x360 = 93600. For use in processing (neural network, etc.)
3) MatAv - 260x360, 2D matrix “Spatial Averaging Algorithm” signal, matrix at the end of the hour interval (that is, only one, the last point of the hour). For image presentation only.
4) ImgAv - 93600x46 Matrix of “Spatial Averaging Algorithm”, 260x360 = 93600 and 46 - number of samps per hour, (after the temporal smoothing algorithm). That is, not the last point, but all points for the current hour. For use in processing (neural network, etc.)
5) MatSig - 260x360, 2D matrix “Subpixel Correlation Algorithm” signal, matrix at the end of the hour interval (that is, only one, the last point of the hour). For image presentation only.
6) ImgSig - 93600x46 Matrix of “Subpixel Correlation Algorithm”, 260x360 = 93600 and 46 - the number of samps per hour (after the temporal smoothing algorithm). That is, not the last point, but all points for the current hour. For use in processing (neural network, etc.)
7-8) PositionX, PositionY - 1x93600 vectors connecting the size of signals 93600 with points in the XY plane (260x360).  
9-10) "t2" - is the time vector (in hours) from the beginning to the end of the experiment. For example 17 hours. Since each file takes a short time interval (1 hour), the "tind" variable indicates the inexes that need to be taken from the "t2" vector for this time window.

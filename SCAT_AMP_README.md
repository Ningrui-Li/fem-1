Scat amp

 All I did for this was change the call that sets phantom.amplitude to randn()
 and pulled it outside of the for loop so that it only happens once in the
 script. I’ve been running sims with a large scatterer mesh (~2 million
 scatterers) and as far as I can tell it seems to work (the RF lines look fine
 and motion tracking with NCC works as expected). The mean of the RF lines is
 somewhere around -1.7e-26, which I assume is so small that it can be counted
 as zero-mean (although Doug will probably have to double check this). Not sure
 what could be going on with your calc_scat, we are using the older version of
 Field so maybe some bug was introduced further down the line?  Here is the
 call:

% Insert amplitudes for scatterers

phantom.amplitude = randn(PPARAMS.N,1);

# waves
MATLAB code to analyze distances between maxima of waves on liquid films

Rey0 = [10 12 20 22 24 26 28 32 36 40];  % Reynolds number
bet0 = [60 75 90]; 
 
% analysis window
xi = [900 700  1000];
xf = [1400 1500 2000];
 
figure;
for i = 1:numel(bet0)
    ReS  = [];
    d = [];
    lambda = [];
    
    for j = 1:numel(Rey0);
        %% Driving Re
        ReD = Rey0(j)*sin(bet0(i)*pi/180);
        
        %% load data from files
        filename2 = ['Bet_',int2str(bet0(i)),'_Rey_',int2str(Rey0(j))];
        load(filename2)
        
        %define data set to be analysed
        
        D = h_dat(end,xi(i):xf(i));
        x = x(xi(i):xf(i));
        
        %find locations of consecutive minima
        [pospks,locks] = findpeaks(D); %find all the maximums
        [~,locksN] = findpeaks(-D);    %find only the location of all the minimums
        [~,glob] = max(pospks);      %find the location of the (first)global maximum
        glob = locks(glob);
        minIdDs = locksN(locksN > glob); %find the indexes of all the minimums that are after the global maximum
        minIdD = minIdDs(1); % take the first min after the global max

        
        %find locations of consecutive maxima
        %[pospks,locks] = findpeaks(D); %find all the maximums
        %[~,locksN] = findpeaks(-D);    %find only the location of all the minimums
        %[~,glob] = max(pospks);      %find the location of the (first)global maximum
        %glob = locks(glob);
        maxIdDs = locks(locks > glob); %find the indexes of all the maximumss that are after the global maximum
        maxIdD = maxIdDs(1); % take the first max after the global max

        
        
        % definitions for graph
        
        d = [d -(x(glob) - x(minIdD))];  %difference between global max and next min
        lambda = [lambda -(x(glob) - x(maxIdD))];      %difference between global max and next max
        ReS  = [ReS ReD];
        
        % plot results
        
        
        
    end
   % hold all;% 
        subplot(1,3,1);  plot(ReS,d, 'o'); hold all;
        subplot(1,3,2);  plot(ReS,lambda, 'o'); hold all;
        subplot(1,3,3);  plot(ReS,d./lambda, 'o'); hold all;
        
        drawnow
end

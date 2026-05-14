% =========================================================
% DRONE ALTITUDE STABILIZATION USING PID CONTROL
% MATLAB + SIMULINK AUTO GENERATION
% =========================================================

clc;
clear;
close all;

%% =========================================================
% MODEL NAME
%% =========================================================

modelName = 'Drone_Altitude_Control7';

%% =========================================================
% CLOSE MODEL IF ALREADY OPEN
%% =========================================================

if bdIsLoaded(modelName)
    close_system(modelName,0);
end

%% =========================================================
% CREATE NEW SIMULINK MODEL
%% =========================================================

new_system(modelName);
open_system(modelName);

%% =========================================================
% ADD BLOCKS
%% =========================================================

% STEP INPUT BLOCK
add_block('simulink/Sources/Step', ...
    [modelName '/Step Input'], ...
    'Position',[30 100 60 130]);

% ERROR SUMMATION BLOCK
add_block('simulink/Math Operations/Sum', ...
    [modelName '/Error Sum'], ...
    'Inputs','+-', ...
    'Position',[120 100 150 130]);

% PID CONTROLLER BLOCK
add_block('simulink/Continuous/PID Controller', ...
    [modelName '/PID Controller'], ...
    'P','42', ...
    'I','24', ...
    'D','18', ...
    'Position',[220 85 340 145]);

% WIND DISTURBANCE BLOCK
add_block('simulink/Sources/Step', ...
    [modelName '/Wind Disturbance'], ...
    'Time','5', ...
    'Before','0', ...
    'After','1.5', ...
    'Position',[250 220 280 250]);

% DISTURBANCE SUM BLOCK
add_block('simulink/Math Operations/Sum', ...
    [modelName '/Disturbance Sum'], ...
    'Inputs','++', ...
    'Position',[400 100 430 130]);

% TRANSFER FUNCTION BLOCK
add_block('simulink/Continuous/Transfer Fcn', ...
    [modelName '/Drone Dynamics'], ...
    'Numerator','[1]', ...
    'Denominator','[1 2 5]', ...
    'Position',[500 85 650 145]);

% SCOPE BLOCK
add_block('simulink/Sinks/Scope', ...
    [modelName '/Scope'], ...
    'Position',[760 95 790 125]);

%% =========================================================
% SET PARAMETERS
%% =========================================================

% DESIRED ALTITUDE
set_param([modelName '/Step Input'], ...
    'Time','0', ...
    'Before','0', ...
    'After','10');

% SIMULATION STOP TIME
set_param(modelName,'StopTime','20');

%% =========================================================
% CONNECT BLOCKS
%% =========================================================

% STEP INPUT TO ERROR SUM
add_line(modelName, ...
    'Step Input/1','Error Sum/1');

% ERROR SUM TO PID
add_line(modelName, ...
    'Error Sum/1','PID Controller/1');

% PID TO DISTURBANCE SUM
add_line(modelName, ...
    'PID Controller/1','Disturbance Sum/1');

% WIND DISTURBANCE CONNECTION
add_line(modelName, ...
    'Wind Disturbance/1','Disturbance Sum/2');

% DISTURBANCE SUM TO DRONE
add_line(modelName, ...
    'Disturbance Sum/1','Drone Dynamics/1');

% DRONE OUTPUT TO SCOPE
add_line(modelName, ...
    'Drone Dynamics/1','Scope/1');

% FEEDBACK CONNECTION
add_line(modelName, ...
    'Drone Dynamics/1','Error Sum/2', ...
    'autorouting','on');

%% =========================================================
% SAVE MODEL
%% =========================================================

save_system(modelName);

%% =========================================================
% RUN SIMULATION
%% =========================================================

simOut = sim(modelName);

disp('=================================')
disp('SIMULATION COMPLETED SUCCESSFULLY')
disp('=================================')

%% =========================================================
% CONTROL SYSTEM ANALYSIS
%% =========================================================

% TRANSFER FUNCTION
G = tf([1],[1 2 5]);

% PID CONTROLLER
C = pid(42,24,18);

% CLOSED LOOP SYSTEM
T = feedback(C*G,1);

%% =========================================================
% STEP RESPONSE
%% =========================================================

figure;
step(T);
grid on;
title('Closed Loop Step Response');

%% =========================================================
% PERFORMANCE METRICS
%% =========================================================

info = stepinfo(T);

disp(' ')
disp('===== PERFORMANCE METRICS =====')
disp(info)

%% =========================================================
% DISTURBANCE ANALYSIS
%% =========================================================

t = 0:0.01:20;

u = ones(size(t));

d = zeros(size(t));

for i = 1:length(t)

    if t(i) >= 5
        d(i) = 1.5;
    end

end

input_signal = u + d;

[y,t] = lsim(T,input_signal,t);

%% =========================================================
% DISTURBANCE RESPONSE GRAPH
%% =========================================================

figure;
plot(t,y,'LineWidth',2);

grid on;

title('Drone Response with Wind Disturbance');
xlabel('Time (seconds)');
ylabel('Altitude');

%% =========================================================
% ROOT LOCUS
%% =========================================================

figure;
rlocus(G);
grid on;
title('Root Locus');

%% =========================================================
% BODE PLOT
%% =========================================================

figure;
bode(G);
grid on;
title('Bode Plot');

%% =========================================================
% POLE ANALYSIS
%% =========================================================

poles = pole(T);

disp(' ')
disp('===== CLOSED LOOP POLES =====')
disp(poles)

%% =========================================================
% STABILITY CHECK
%% =========================================================

if all(real(poles) < 0)

    disp(' ')
    disp('SYSTEM IS STABLE')

else

    disp(' ')
    disp('SYSTEM IS UNSTABLE')

end

%% =========================================================
% OPEN SIMULINK MODEL
%% =========================================================

open_system(modelName);

disp(' ')
disp('CLICK RUN BUTTON INSIDE SIMULINK TO VIEW SCOPE OUTPUT')

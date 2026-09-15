# IIR-FILTER-DESIGN
# EXP 3 A: DESIGN OF LOW PASS BUTTERWORTH FILTER USING BILINEAR TRANSFORMATION TECHNIQUE

# AIM: 

To perform design of Butterworth Filter Using Impulse Invariant and Bilinear Transformation Techniques using SCILAB.

# APPARATUS REQUIRED: 
PC installed with SCILAB. 

# PROGRAM: 
```
clc;
close;

// --- 1. GIVEN SPECIFICATIONS (Linear Gain) ---
wp = 0.35 * %pi; // Digital Passband Frequency (Radians)
ws = 0.7 * %pi;  // Digital Stopband Frequency (Radians)
Ap = 0.6;        // Passband Gain (Linear, NOT dB)
As = 0.1;        // Stopband Gain (Linear, NOT dB)
T = 1;           // Sampling Time

disp(Ap, 'Passband Gain (Ap) =');
disp(As, 'Stopband Gain (As) =');

// --- 2. PRE-WARPING (Bilinear Transformation) ---
// BLT uses the tangent function for frequency mapping
omegap = (2/T) * tan(wp/2);
omegas = (2/T) * tan(ws/2);

disp(omegap, 'Analog Passband Freq (Omega_p) =');
disp(omegas, 'Analog Stopband Freq (Omega_s) =');

// --- 3. CALCULATE FILTER ORDER (N_T) ---
// Using the exact linear gain formula from your notes
num_term = log10( ((1/As^2) - 1) / ((1/Ap^2) - 1) );
den_term = log10(omegas / omegap);
N_T = 0.5 * (num_term / den_term);

disp(N_T, 'Calculated Exact Order (N_T) =');
N = ceil(N_T);
disp(N, 'Round off value of N =');

// --- 4. CALCULATE ANALOG CUT-OFF FREQUENCY ---
// Anchored to the stopband formula from your notes
omegac = omegas / ( ((1/As^2) - 1)^(1/(2*N)) );
disp(omegac, 'Analog Cut-off Frequency (Omega_c) =');

// --- 5. ANALOG TRANSFER FUNCTION H(S) ---
disp('Analog LPF Transfer function H(S) =');
hs = analpf(N, 'butt', [0,0], omegac);
disp(hs);

// --- 6. BILINEAR TRANSFORMATION (S -> Z) ---
z = poly(0, 'z'); // Define variable 'z' as a polynomial

// horner() evaluates the polynomial by substituting S with the BLT equation
Hz = horner(hs, (2/T) * ((z - 1)/(z + 1)));

// Clean up any microscopic floating-point rounding errors
Hz = clean(Hz); 

disp('Digital LPF Transfer function H(Z) =');
disp(Hz);

// --- 7. FREQUENCY RESPONSE PLOT ---
HW = frmag(Hz, 512); 
w = 0 : %pi/511 : %pi; 

plot(w/%pi, abs(HW));
xlabel('Normalized Digital Frequency (w / \pi)');
ylabel('Magnitude');
title('Butterworth IIR LPF (Bilinear Transformation - Linear Gain)');
```

# MANUAL CALCULATION: 

<img width="977" height="1545" alt="image" src="https://github.com/user-attachments/assets/78323d6b-612f-4722-aab2-cac7efc205c3" />
<img width="898" height="1599" alt="image" src="https://github.com/user-attachments/assets/d249df04-bcdf-4eb3-bfe2-736b40e7d3de" />
<img width="1031" height="1516" alt="image" src="https://github.com/user-attachments/assets/09f86a59-d85f-4dae-88ac-0a36ba3719f6" />
<img width="1024" height="1600" alt="image" src="https://github.com/user-attachments/assets/25cef674-acf5-40f4-a97a-13925feced19" />
<img width="1013" height="1599" alt="image" src="https://github.com/user-attachments/assets/21d31458-6e73-4213-8665-500c50a66548" />

# SAMPLE OUTPUT:

<img width="1917" height="892" alt="image" src="https://github.com/user-attachments/assets/01de4fd5-fc09-44a1-b328-f271f79d1040" />

# RESULT: 

Thus, design of Butterworth Low pass IIR filter waveforms were plotted and output was verified.


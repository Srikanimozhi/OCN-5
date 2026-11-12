# Free-Space Optical Communication (FSO) Implementation

## Aim: 
To implement and analyze a free space optical communication link using a laser/LED transmitter and photodiode/APD receiver.

## Apparatus/Software Required:
Python Colab

## Theory:
	FSO uses light beams to transmit data through free space instead of fiber.
	Performance depends on distance, alignment, and atmospheric attenuation.
	Received power decreases exponentially with distance.

## Formula:
Loss=10⋅〖log⁡〗_10 (P_in/P_out )

## 1
## Setup the Transmitter
Info
Prepare the optical source for transmission.
	Use a Laser/LED source
	Mount it on an optical bench
	Ensure stable power supply

## 2
## Align the Optical Path
Warning
Proper alignment ensures maximum received signal.
	Place transmitter and receiver in line of sight
	Adjust using lenses/mirrors if needed
	Minimize beam divergence

## 3
## Connect the Receiver
Setup the photodiode/APD to detect the optical signal.
	Mount detector opposite to transmitter
	Connect to oscilloscope for signal observation
	Record received power

## 4
## Measure Received Power
Collect data at different distances.
Loss = 10 log10(Pin / Pout)
	Vary distance (1 m, 5 m, 10 m, etc.)
	Note received power in µW
	Calculate link loss

## 5
## Plot Graphs
Visualize performance of FSO link.
	Plot Received Power vs Distance
	Observe exponential decay trend
	Compare with theoretical model

## 6
## Conclude Results
Success
Summarize findings of the experiment.
	Reliable communication up to ~10 m indoors
	Loss increases with distance
	Alignment critical for performance

## Sample Output

<img width="630" height="467" alt="image" src="https://github.com/user-attachments/assets/0ec8bb24-ddfb-4d3a-b0f2-a5b661798ad4" />
<img width="672" height="466" alt="image" src="https://github.com/user-attachments/assets/2a113e54-2433-4072-ba62-16539939e93a" />
<img width="672" height="461" alt="image" src="https://github.com/user-attachments/assets/448e0d3f-a834-4e14-953f-4d429b7a9996" />

## Python Code
```

import numpy as np
import matplotlib.pyplot as plt

# Parameters
lambda_nm = 1550
lambda0 = 550
L = 2
V_range = np.linspace(0.1, 20, 100)
R_range = np.linspace(0, 50, 100)

# Kruse Model
def kruse_q(V):
    if V > 50:
        return 1.6
    elif V > 6:
        return 1.3
    else:
        return 0.585 * (V ** (1/3)) + 0.34

# Atmospheric Attenuation
q_values = np.array([kruse_q(v) for v in V_range])
alpha = (3.91 / V_range) * ((lambda_nm / lambda0) ** (-q_values))
tau = 4.343 * alpha * L

# Rain Attenuation
A_rain = 1.076 * (R_range ** 0.67)

# Graph 1: Attenuation vs Visibility
plt.figure()
plt.plot(V_range, tau)
plt.xlabel("Visibility (km)")
plt.ylabel("Atmospheric Attenuation (dB)")
plt.title("FSO Attenuation vs Visibility (Kruze Model)")
plt.grid()
plt.show()

# Graph 2: Rain Attenuation
plt.figure()
plt.plot(R_range, A_rain)
plt.xlabel("Rain Rate (mm/hr)")
plt.ylabel("Rain Attenuation (dB/km)")
plt.title("Rain Attenuation in FSO Link")
plt.grid()
plt.show()

# Graph 3: Attenuation vs Wavelength
lambda_range = np.linspace(800, 1600, 100)
tau_lambda = []

for lam in lambda_range:
    q = kruse_q(2)
    alpha_l = (3.91 / 2) * ((lam / lambda0) ** (-q))
    tau_l = 4.343 * alpha_l * L
    tau_lambda.append(tau_l)

plt.figure()
plt.plot(lambda_range, tau_lambda)
plt.xlabel("Wavelength (nm)")
plt.ylabel("Attenuation (dB)")
plt.title("Attenuation vs Wavelength (Fog Condition)")
plt.grid()
plt.show()
```


## Output
<img width="571" height="455" alt="image" src="https://github.com/user-attachments/assets/b17fcd8d-e78f-4772-b79e-7a5c2b6509cd" />

<img width="562" height="455" alt="image" src="https://github.com/user-attachments/assets/257e3a67-9224-417f-8e5d-a5f54b884fce" />

<img width="563" height="455" alt="image" src="https://github.com/user-attachments/assets/5fce69f3-74c0-42ed-bc5a-40ae132659fa" />


## Result
Reliable FSO link up to ~10 m indoors; beyond that, signal quality degrades significantly.

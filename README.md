
# Anomaly Modeling in Connected and Automated Vehicles (CAV)

## Overview

This repository contains code and documentation for anomaly modeling in Connected and Automated Vehicles (CAV). The focus is on identifying and simulating sensor anomalies to evaluate the robustness and resilience of sensor fusion systems. The code is based on the SPMD dataset, which includes in-vehicle speed (sensor 1), GPS speed (sensor 2), and in-vehicle acceleration (sensor 3).

## Dataset

The SPMD dataset used for this experiment includes:
- **Sensor 1:** In-vehicle speed (s)
- **Sensor 2:** GPS speed (GPSS)
- **Sensor 3:** In-vehicle acceleration (Ax)

Due to the lack of publicly available datasets featuring anomalous sensor behaviors due to attacks, we injected anomalies into the SPMD datasets through simulations. Our anomalous/attack scenario is almost like a real-time scenario because we used a real-time SPMD dataset from the United States Department of Transportation (USDOT) and the anomalies we introduced followed the distributions of real-time anomaly cases. The anomalies modeled and injected into the data are instant, bias, and gradual drift anomalies.

## Attack Simulation

In our attack simulation experiment, we argued that in real-life scenarios, simultaneous attacks could perturb multiple sensors concurrently, unlike the independent perturbations assumed in previous studies. In this case, malicious actors might intentionally compromise multiple sensor readings simultaneously, introducing spatial anomalies and presenting a more sophisticated threat.

The attack modeling involves the simultaneous perturbation of sensor readings (s, GPSS, and Ax) for 10-time steps when certain conditions are met. This perturbation introduces spatial anomalies, challenging the robustness and resilience of sensor fusion systems.

## Details of the attack algorithm in Python 
def apply_anomalies(dfdata, anomaly_type):
    # Copy sensor data to create anomaly-applied versions
    In_vehicle_anomaly = dfdata['InVehicle_Longitudinal_Speed'].values.copy()
    GPS_Speed_anomaly = dfdata['GPS_Speed'].values.copy()
    Acc_anomaly = dfdata['InVehicle_Longitudinal_Accel'].values.copy()
    class_output = np.zeros(len(dfdata))

    for t in range(len(dfdata)):  # Loop over the dataset length
        if np.random.uniform(0, 1) <= ALPHA:  # Determine if anomaly should be applied based on ALPHA
            zeta = np.random.uniform(0, 1)  # Random value to select the sensor

            # Determine the actual anomaly type to be applied
            ANOMALY_TYPE_ACTUAL = anomaly_type
            if anomaly_type == 5:  # Mixed anomaly
                ANOMALY_TYPE_ACTUAL = np.random.randint(1, NUM_TYPES + 1)

            for i in range(NUM_SENSORS):
                if i / NUM_SENSORS <= zeta < (i + 1) / NUM_SENSORS:
                    # Instant Anomaly
                    if ANOMALY_TYPE_ACTUAL == 1:
                        anomaly = SCALE * np.random.normal(0, 0.1)
                        if i == 0:
                            In_vehicle_anomaly[t] += anomaly
                        elif i == 1:
                            GPS_Speed_anomaly[t] += anomaly
                        elif i == 2:
                            Acc_anomaly[t] += anomaly
                        class_output[t] = 1  # Mark anomaly occurrence
                        break

                    # Constant Anomaly
                    elif ANOMALY_TYPE_ACTUAL == 2:
                        anomaly = np.random.uniform(0, MAX_MAG_C)
                        end_idx = min(t + DURATION_C, len(dfdata))  # Prevent index overflow
                        if i == 0:
                            In_vehicle_anomaly[t:end_idx] += anomaly
                        elif i == 1:
                            GPS_Speed_anomaly[t:end_idx] += anomaly
                        elif i == 2:
                            Acc_anomaly[t:end_idx] += anomaly
                        class_output[t:end_idx] = 1  # Mark anomaly occurrence
                        break

                    # Gradual Drift Anomaly
                    elif ANOMALY_TYPE_ACTUAL == 3:
                        GD = np.linspace(0, MAX_MAG_G, num=DURATION_G)
                        end_idx = min(t + DURATION_G, len(dfdata))  # Prevent index overflow
                        if i == 0:
                            In_vehicle_anomaly[t:end_idx] += GD[:end_idx - t]
                        elif i == 1:
                            GPS_Speed_anomaly[t:end_idx] += GD[:end_idx - t]
                        elif i == 2:
                            Acc_anomaly[t:end_idx] += GD[:end_idx - t]
                        class_output[t:end_idx] = 1  # Mark anomaly occurrence
                        break

                    # Bias Anomaly
                    elif ANOMALY_TYPE_ACTUAL == 4:
                        anomaly = np.random.uniform(0, MAX_MAG_B)
                        end_idx = min(t + DURATION_B, len(dfdata))  # Prevent index overflow
                        if i == 0:
                            In_vehicle_anomaly[t:end_idx] += anomaly
                        elif i == 1:
                            GPS_Speed_anomaly[t:end_idx] += anomaly
                        elif i == 2:
                            Acc_anomaly[t:end_idx] += anomaly
                        class_output[t:end_idx] = 1  # Mark anomaly occurrence
                        break

    # Create a new DataFrame containing the anomalies
    df_anomaly = dfdata.copy()
    df_anomaly['InVehicle_Longitudinal_Speed'] = In_vehicle_anomaly
    df_anomaly['GPS_Speed'] = GPS_Speed_anomaly
    df_anomaly['InVehicle_Longitudinal_Accel'] = Acc_anomaly
    df_anomaly['Class_Output'] = class_output

    return df_anomaly


# Sample usage with dummy DataFrame dfdata
# dfdata = pd.read_csv("your_data.csv")  # Load your data

# Apply anomalies and save the output
df_anomaly = apply_anomalies(dfdata, ANOMALY_TYPE)

# Define the filename using string formatting
filename = f"data_with_anomaly_type_25{ANOMALY_TYPE}.csv"

# Save the DataFrame to the CSV file
df_anomaly.to_csv(filename, index=False)

# Download the CSV file
files.download(filename)


<br>Preferred Citation:


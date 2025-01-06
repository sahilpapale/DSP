# DSP and Architecture

The Digital Signal Processing (DSP) and Architecture course provided a comprehensive understanding of signal analysis and manipulation using digital techniques. We studied various types of filters, including Infinite Impulse Response (IIR) filters and digital filters, focusing on their design, implementation, and applications. Additionally, we explored programmable digital processors, learning how they can be used to efficiently process signals in real-time. This course combined theoretical concepts with practical skills, preparing us to tackle real-world signal processing challenges.
## Course Project
We had designed and implemented a **Low Pass Butterworth Filter**
### Team
|Sl.no|Name|
|---|----|
|1|Sahil Papale|
|2|Vishal Deshpande|
|3|Tushar Punywant|
|4|Sagar Halkarni|
### Guide:Prof.Amit

Processor used:**TMS320C6748**.<br>
Sofware Used:**CodeComposerStudio IDE v7**.<br>
Development Kit used:**TMS320C6748LCDK**.

### About Development Kit
 The TMS320C6748 DSP development kit (LCDK) is a scalable platform that breaks down 
development barriers for applications that require embedded analytics and real-time signal 
processing, including biometric analytics, communications and audio. The low-cost LCDK 
will also speed and ease your hardware development of real-time DSP applications. This 
new board reduces design work with freely downloadable and duplicable board schematics 
and design files. A wide variety of standard interfaces for connectivity and storage enable 
you to easily bring audio, video and other signals onto the board. 

### CODE
**Question**:Write a C program to implement a 4th-order low-pass Butterworth filter with a cutoff frequency of 5 kHz and a sampling rate of 48 kHz. The program should generate a 1 kHz sine wave as the input signal, apply the Butterworth filter, and print the filtered output values. Use precomputed coefficients for the Butterworth filter and ensure the program handles buffer processing efficiently

    #include <stdio.h>
    #include <math.h>
    #include <stdlib.h> // For dynamic memory allocation if needed

    // Define filter parameters
    #define FILTER_ORDER 4
    #define SAMPLE_RATE 48000 // 48 kHz
    #define CUTOFF_FREQ 5000  // 5 kHz

      // Define buffer size for processing
      #define BUFFER_SIZE 8

      // Define M_PI if not available
      #ifndef M_PI
      #define M_PI 3.14159265358979323846
      #endif

      // Hardcoded filter coefficients for a 4th-order Butterworth low-pass filter
      const float a[FILTER_ORDER + 1] = {1.0000, -3.1806, 3.8612, -2.1122, 0.4383};
      const float b[FILTER_ORDER + 1] = {0.0004, 0.0017, 0.0026, 0.0017, 0.0004};

      // Input and output buffers
      float x[BUFFER_SIZE];  // Input signal
      float y[BUFFER_SIZE];  // Output signal

      // History buffers for filtering
      float x_hist[FILTER_ORDER + 1] = {0};
      float y_hist[FILTER_ORDER + 1] = {0};


      // Function prototype
    void butterworth_filter(float *input, float *output, int buffer_size);

    int main() {

    // Declare loop variable
    int i;

    // Generate a test signal: 1 kHz sine wave
    for (i = 0; i < BUFFER_SIZE; i++) {
        x[i] = sin(2 * M_PI * 1000 * i / SAMPLE_RATE); // 1 kHz sine wave
    }

    // Apply the Butterworth filter
    butterworth_filter(x, y, BUFFER_SIZE);


    // Print the output signal
    for (i = 0; i < BUFFER_SIZE; i++) {
        printf("y[%d] = %f\n", i, y[i]);
    }
    return 0;
    }


    // Function to apply the Butterworth filter

    void butterworth_filter(float *input, float *output, int buffer_size) 
    {

    int i, j; // Declare loop variables
    for (i = 0; i < buffer_size; i++) {
        // Update the input history
        x_hist[0] = input[i]; // Current input sample
        y_hist[0] = 0;        // Initialize current output

        // Apply the filter equation
        for (j = 0; j <= FILTER_ORDER; j++) {
            y_hist[0] += b[j] * x_hist[j]; // Feedforward part
            if (j > 0) {
                y_hist[0] -= a[j] * y_hist[j]; // Feedback part
            }
        }
        // Shift the history buffers for the next sample
        for (j = FILTER_ORDER; j > 0; j--) {
            x_hist[j] = x_hist[j - 1];
            y_hist[j] = y_hist[j - 1];
        }
        // Store the output sample
        output[i] = y_hist[0];
    }
 ## Results
### On Development KIT
 ![DSPOUTPUT](https://github.com/user-attachments/assets/cb7db52c-624c-4f7e-bce8-dbcd2c12c4da)
 ### Solving Manually
y[0] = 0.000000 <br>
y[1] = 0.130526 <br>
y[2] = 0.258819 <br>
y[3] = 0.382683 <br>
y[4] = 0.500000 <br>
y[5] = 0.608761 <br>
y[6] = 0.707107 <br>
y[7] = 0.793353
  ### Graph
  ![WhatsApp Image 2025-01-06 at 9 39 28 PM](https://github.com/user-attachments/assets/ed3693a0-5277-4802-9b8f-3b4eda50242f)

    

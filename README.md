# PAM_Modulation
## Introduction:
In a PAM system, the transmitted signal has the following form:

![image](https://user-images.githubusercontent.com/125180530/227782601-cd7da580-8316-4b31-b769-50ccc9055fe4.png)

Consider that the sequence {ak} is WSS and independent. The power spectrum density of this signal is:

![image](https://user-images.githubusercontent.com/125180530/227782789-fd2ca72b-6662-4d14-99cd-56dff73a90e9.png)

![image](https://user-images.githubusercontent.com/125180530/227782750-9f0ff079-3bf4-41f4-9a7f-7b20d6561f3a.png)

![image](https://user-images.githubusercontent.com/125180530/227782983-b19195db-03ae-4683-963e-a3c6ad970096.png)

Then we define SNR by having the symbol energy:

![image](https://user-images.githubusercontent.com/125180530/227783068-3a44e418-84ff-4b14-853a-78060245215d.png)

In this project the probability of symbols is chosen in the way that we have:

![image](https://user-images.githubusercontent.com/125180530/227783169-75ed8280-3942-4354-9ff0-b6fbec6a4ad6.png)

## Generate and Transfer Symbols with 4-PAM Modulation:
* First, we generate a raised-cosine pulse for the following parameters in the interval: [-6T,6T]

![image](https://user-images.githubusercontent.com/125180530/227783347-7f71f980-79b7-4320-a3e3-d549ab40b870.png)

The formula for the raised cosine is:

![image](https://user-images.githubusercontent.com/125180530/227783768-1d42cc92-33f9-4695-887a-339bbb9024e5.png)

* With the use of 4-PAM modulation we generate A, B, C, and D symbols with the probabilities: 0.1, 0.4, 0.4, 0.1. Notice that the amplitude of the pulse for each symbol should obey the following rule:

![image](https://user-images.githubusercontent.com/125180530/227783701-09bd8e64-6856-40cd-83a3-811b3663a9ab.png)

With the use of the 'rand' function, we generate a vector of values. The values are between 0 and 1. So, we divide this interval by 4. We can say:

![image](https://user-images.githubusercontent.com/125180530/227784114-ad98c781-6e0f-4ee1-bd15-4213f20089de.png)

So, with the above conditions, we change the vector 'samples' to 'modulated_symbols'. 

* Now, we convolve the vector in the previous step with the generated pulse to reach the 'transmitted_signal'. (before convolving the pulse with the vector, we have to upsample the vector. It's the same way we do in the 'Transmitter_and_Receiver_Simulation_in_Digital_Communication' repository). 

* Then, create the SNR values vector both in dB and linear domains. The value of the energy for each symbol can be calculated with the following formula:

![image](https://user-images.githubusercontent.com/125180530/227784420-25b5e90b-fc71-4fe2-b693-a128217b56c5.png)

After we calculate the energy, we generate eta:

![image](https://user-images.githubusercontent.com/125180530/227784473-28bd53e2-a834-4517-9315-d0644e17ef1a.png)

Then, after generating the noise vector with the 'randn' function, we add it to the previous vector and generate 'received_signal'.

## Symbol Revelation with MAP and ML:
* In this part first of all we sample from the 'received_signal'. Then, for each receiver (MAP and ML), we calculate decision threshold levels.

* For each case, generate 'detected_symbols' by making a comparison between 'samples' values with threshold levels. 

* In the final step, after generating 'detected_symbols', for both of the receivers we calculate the error probability and plot the values vs SNR_dB. 

### MAP Receiver
For determining the threshold levels in this case, we have to find the intersection of the curves of each symbol that are shown in the following figure:

![image](https://user-images.githubusercontent.com/125180530/227784943-37ab96a9-34c0-42cd-8b42-1268d23f5569.png)

So, we can write:

![image](https://user-images.githubusercontent.com/125180530/227784972-ab6502a1-022f-4193-9d03-4675270cfcaa.png)

![image](https://user-images.githubusercontent.com/125180530/227784985-c1e2ab22-1612-430f-8fbd-aa57713a9021.png)

![image](https://user-images.githubusercontent.com/125180530/227785003-c3c9da39-9fcc-434e-8f9c-5d1dbc81145e.png)

The plot of the error probability for this receiver is as follows:

![image](https://user-images.githubusercontent.com/125180530/227785241-9355b4af-d650-4c19-885d-4f8b1798d39b.png)

### ML Receiver
We use this receiver in case the prior probabilities are unknown to us. So, we consider the probabilities to be the same and we expect that the threshold levels be placed in the middle of the average of the curves of each symbol. It's shown in the following figure:

![image](https://user-images.githubusercontent.com/125180530/227785399-eef301fc-5e21-4623-9d82-c9e690273f9c.png)

So, we have:

![image](https://user-images.githubusercontent.com/125180530/227785425-a16bb62a-db8b-420d-9b9a-2a38ce1460f1.png)

The plot of the error probability for this receiver is as follows:

![image](https://user-images.githubusercontent.com/125180530/227785469-50fc8c65-26f1-413e-a024-89b3a19952fd.png)

For better comparison, we plot the error probabilities of both receivers together:

![image](https://user-images.githubusercontent.com/125180530/227785501-51b5c6ed-5f0a-4e88-a76c-6bbe0c5c0948.png)

### Conclusion
As in ML we don't have the probabilities and we consider them to be the same as each other, we expect to have higher error values in this case. In MAP, because for each value of SNR we calculate the corresponding eta and noise, the values of threshold levels have different values because of the noise variance values. So, we expect that the MAP receiver has lower error probabilities. 

Another point is that the higher the value of SNR, the closer the graphs of both receivers. So, we can come to the conclusion that for greater values of SNR, the error probabilities of both methods are approximately the same. 

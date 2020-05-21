# SSTA_Learning_Project
### This project is for SSTA team in EE595 at University of Southern California. 

## Members and Collaborative Team
Project Instructors: Shahin Nazarian and Mohammad Saeed Abrishami

Team Members:
* [Zhiyu Chen(Andrew)](https://github.com/Zhiyu-Chen-Github)
* [Hsu-Cheng Cheng(Alex)](https://github.com/HCC7952889662)
* [Harmanpreet Singh Kalsi](https://github.com/hkalsi-usc)
* [Lingjun Sun](https://github.com/sunlingjun)

Collaborative Team : USC ATPG Team

## Fast Test of SSTA calculation:
```Bash
python3 main.py <Library File PATH> <Circuit File PATH>
```
Ex:
```Bash
python3 main.py ./tech10nm.sstalib ./circuit/c17.ckt658
```
## SSTA System Diagram
![Diagram](/images/System_Diagram.png)
<p align="right">by ZhiYu</p>

## PDF(Probability Density Function) Sampling Diagram
![Diagram](/images/sample.png)

In our implementation, we sample the PDF created by a given PDF type, mean value and std, by using the term called "Sample_Dist", which displays the distance between 2 sample points. The 'Sample_Dist' will become a constant during the whole circuit analysis, and this will offer huge advantages over SUM and MAX implementation. However, though this way gives us a promising way to decrease the distortion of the PDF, the total number of sampling points will become extremely large when the std become huge, thus we need the data_shrink() function to drop some delay points whose probability are lower than P_tolerance.


## SUM Function
![Diagram](/images/sum.png)

To implement the SUM function, we first find out the range of delays that are possible in SUM Function. The lowest delay of the SUM will be the SUM of minimum of delay of 2 PDFS and the highest delay of the SUM will be the SUM of maximum of delay of 2 PDFS. After getting the range, we can acquire all possible delays of SUM by dividing the range with Sample_Dist. Next, we used 2 pointers to implement Convolution. 'P1' is to point at where the tail of the second input, and the other pointer is to calculate the overlapped delays.

## MAX Function
![Diagram](/images/max_f.png)

To implement the MAX function, just like the SUM, we first find out the range of delays that are possible in MAX Function. The lowest delay of the SUM will be the max of minimum delay of 2 PDFS and the highest delay of the MAX will be the max of maximum delay of 2 PDFS. After getting the range, we can acquire all possible delays of MAX by dividing the range with Sample_Dist. Next, for every delay in the range, we multiply the probabilityof that point to the probability of sum of the point that are smaller than it. We have to do it twice if that point exists in 2 Inputs.

## PDF(Probability Density Function) Object
In the PDF object, the constructor will create a PDF according to the given 5 arguments.
1. mu : mu is the mean value of a giveen data.
2. sigma: sigma is a square of std of a given data.
3. form: form represents which kinds of PDF. 
4. sample_dist: The distance between 2 sampling points.

## Basic Functions in PDF Object:
1.SUM(PDF) : return the result of the summation of 2 PDFs. Return Type: PDF Object

2.MAX(PDF) : return the result of the maximum of 2 PDFs. Return Type: PDF Object 

3.NORM(mu,sigma,size) : creating the normal PDF data points and stored in PDF Object according to the mu, sigma and size given by the user.

4.decimal_place_generator(): return the number of the decimal place of the Sample_Dist

5.mu(): return the mean value of the PDF Object.

6.std(): return the standard deviation value of the PDF Object.

7.plot(): plot the PDF Object.

## Speed Improved Functions in PDF Object:
data_shrink(): This function delete delay data in PDF whose probability is small than P_tolerance and can be ignored. By doing this, we can speed up the simulation due to the decrease of data which is needed to be dealt with.  

## Experimental Functions in PDF Object:
1. MAX_of_SUM(PDF p2, PDF pc): do the MAX(self,pc) ,MAX(p2,pc) and do the SUM of those 2 max results and return the final result. Return Type: PDF Object  

2. SUM_of_MAX(PDF p2, PDF pc): do the SUM(self,pc) ,SUM(p2,pc) and do the MAX of those 2 sum results and return the final result. Return Type: PDF Object
These 2 functions are designed for the experiment in PHASE I project, which is to see whether the results are different in these 2 functions.

## Sample Results for c432 circuit:
![c6288](/images/c432_re.png)

## Reference:
[1]  J.-H. Liu, A.-S. Hong, L. Chen, and C. Chen, “Process-variation statisticalmodeling for vlsi timing analysis,” 04 2008, pp. 730 – 733.

[2]  M.LLC.(1999)MSWindowsNTkerneldescription.[Online].    Available: http://web.archive.org/web/20080207010024/http://www.808multimedia.com/winnt/kernel.htm

[3]  “Grinstead and Snell’s Introduction to Probability,” Tech. Rep., 2006.

[4]  D. Blaauw, K. Chopra, A. Srivastava, and L. Scheffer, “Statistical timinganalysis:  From  basic  principles  to  state  of  the  art,”Computer-AidedDesign of Integrated Circuits and Systems, IEEE Transactions on, vol. 27,pp. 589 – 607, 05 2008.

[5]  “(No   Title).”   [Online].   Available:   http://sportlab.usc.edu/∼mitsushij/benchmarks.html[6]  V.  Khandelwal  and  A.  Srivastava,  “A  general  framework  for  accuratestatistical  timing  analysis  considering  correlations,”  inProceedings -Design Automation Conference.New  York,  New  York,  USA:  ACMPress, 2005, pp. 89–94. [Online]. Available: http://portal.acm.org/citation.cfm?doid=1065579.1065607

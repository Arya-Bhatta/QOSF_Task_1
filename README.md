# QOSF_Task_1
## **Quantum Search on a unstructed database.**
# **Problem Statement:**
Design a quantum circuit that considers as input the following vector of integers numbers: \
**[1,5,7,10]** \
returns a quantum state which is a superposition of indices of the target solution, obtaining in the output the indices of the inputs where two adjacent bits will always have different values.

In this case the output should be: \
1/sqrt(2) * (|01> + |11>), as the correct indices are 1 and 3. 

1 = 0001 \
5 = 0101 \
7 = 0111 \
10 = 1010 

The method to follow for this task is to start from an array of integers as input, pass them to a binary representation and you need to find those integers whose binary representation is such that two adjacent bits are different. Once you have found those integers, you must output a superposition of states where each state is a binary representation of the indices of those integers.
### **Example**
Consider the vector: [1,5,4,2]. \
Pass the integer values to binary numbers that is [001,101,100,010] \
Identify the values whose binary representation is such that two adjacent bits are different, we can see that there are 2 - **101 and 010**, among [001,101,100,010].
Return the linear combination of the indices in which the values satisfying the criterion are found. \

Indices- Corresponding States: \
   0 - 001\
   1 - 101\
   2 - 100\
   3 - 010

Indices are converted to binary states\
|00> - 001\
|01> - 101\
|10> - 100\
|11> - 010

 The answer would be the superposition of the states |01> and |11> or 1/sqrt(2) * (|01> + |11>)


## **Context**
If you’re struggling to find a proper way to solve this task, you can find some suggestions for a possible solution below. This is one way to approach the problem, but other solutions may be feasible as well, so feel free to also investigate different strategies if you see fit!

The key to this task is to use the superposition offered by quantum computing to load all the values of the input array on a single quantum state, and then locate the values that meet the target condition. So, how can we use a quantum computer to store multiple values? A possible solution is using the QRAM (some references: https://arxiv.org/pdf/0708.1879.pdf, https://github.com/qsharp-community/qram/blob/master/docs/primer.pdf).

As with classical computers, in the QRAM information is accessed using a set of bits indicating the address of the memory cell, and another set for the actual data stored in the array. 

For example, if you want to use a QRAM to store 2 numbers that have at most 3 bits, it can be achieved with 1 qubit of address and 3 qubits of data. \
Suppose you have the vector input_2 = [2,7].
In a properly constructed circuit, when the value of the address qubit is |0> the data qubits have value 010 (binary representation of 2) and when it is |1> in the data qubits have value 111 (binary representation of 7).

Given such a structure, you should be able to use Grover’s algorithm in order to obtain the solution to the task. \
You can assume that the input always contains at least two numbers that have alternating bitstrings. \

## Bonus:
Design a general circuit that accepts vectors with random values of size 2^n with m bits in length for each element and finds the state(s) indicated above from an oracle.

# **Idea Behind the Algorithm**
Given a vector of length N(=2^n) containing values of m-bit length, we need to find all the indices of the states which represent states that have a binary representation of alternating 1's and 0's.\
To find all such indices we must somehow 'mark' those states based on their binary representation and use a modifed Grover's algorithm to search for the required indices. Essentially we want to mark our indices based on our data and this is the challenging part.\

After doing a lot of literature survey on this topic, I found some resouces that helped me understand how to search a unstructured classical database quantumly. But the main hindrance was the loading of classical data from the given vector into a memory from which it will accessible to operate quantumly. \
I found such a architecture and then builded upon it to derive this circuit. The different components of this circuit is described briefly below.
## **Circuit Implementation using Quirk**
[link to the circuit](https://algassert.com/quirk#circuit=%7B%22cols%22%3A%5B%5B%22H%22%2C%22H%22%2C%22H%22%5D%2C%5B%22%E2%97%A6%22%2C%22%E2%97%A6%22%2C%22%E2%97%A6%22%2C1%2C1%2C1%2C%22X%22%5D%2C%5B%22%E2%97%A6%22%2C%22%E2%97%A6%22%2C%22%E2%80%A2%22%2C1%2C%22X%22%2C1%2C%22X%22%5D%2C%5B%22%E2%97%A6%22%2C%22%E2%80%A2%22%2C%22%E2%97%A6%22%2C1%2C%22X%22%2C%22X%22%2C%22X%22%5D%2C%5B%22%E2%97%A6%22%2C%22%E2%80%A2%22%2C%22%E2%80%A2%22%2C%22X%22%2C1%2C%22X%22%5D%2C%5B1%2C1%2C1%2C%22%E2%80%A2%22%2C1%2C1%2C1%2C%22X%22%5D%2C%5B1%2C1%2C1%2C1%2C%22%E2%80%A2%22%2C1%2C1%2C1%2C%22X%22%5D%2C%5B1%2C1%2C1%2C1%2C1%2C%22%E2%80%A2%22%2C1%2C1%2C1%2C%22X%22%5D%2C%5B1%2C1%2C1%2C1%2C1%2C1%2C%22%E2%80%A2%22%2C1%2C1%2C1%2C%22X%22%5D%2C%5B1%2C1%2C1%2C1%2C1%2C1%2C1%2C%22%E2%97%A6%22%2C%22%E2%97%A6%22%2C%22%E2%97%A6%22%2C%22%E2%97%A6%22%2C%22X%22%5D%2C%5B1%2C1%2C1%2C1%2C1%2C1%2C1%2C%22%E2%80%A2%22%2C%22%E2%80%A2%22%2C%22%E2%80%A2%22%2C%22%E2%80%A2%22%2C%22X%22%5D%2C%5B1%2C1%2C1%2C%22%E2%80%A2%22%2C1%2C1%2C1%2C%22X%22%5D%2C%5B1%2C1%2C1%2C1%2C%22%E2%80%A2%22%2C1%2C1%2C1%2C%22X%22%5D%2C%5B1%2C1%2C1%2C1%2C1%2C%22%E2%80%A2%22%2C1%2C1%2C1%2C%22X%22%5D%2C%5B1%2C1%2C1%2C1%2C1%2C1%2C%22%E2%80%A2%22%2C1%2C1%2C1%2C%22X%22%5D%2C%5B%22%E2%97%A6%22%2C%22%E2%97%A6%22%2C%22%E2%97%A6%22%2C1%2C1%2C1%2C%22X%22%5D%2C%5B%22%E2%97%A6%22%2C%22%E2%97%A6%22%2C%22%E2%80%A2%22%2C1%2C%22X%22%2C1%2C%22X%22%5D%2C%5B%22%E2%97%A6%22%2C%22%E2%80%A2%22%2C%22%E2%97%A6%22%2C1%2C%22X%22%2C%22X%22%2C%22X%22%5D%2C%5B%22%E2%97%A6%22%2C%22%E2%80%A2%22%2C%22%E2%80%A2%22%2C%22X%22%2C1%2C%22X%22%5D%2C%5B1%2C1%2C1%2C1%2C1%2C1%2C1%2C1%2C1%2C1%2C1%2C%22H%22%5D%2C%5B%22H%22%2C%22H%22%2C%22H%22%5D%2C%5B%22X%22%2C%22X%22%2C%22X%22%2C1%2C1%2C1%2C1%2C1%2C%22X%22%2C1%2C%22X%22%2C%22X%22%5D%2C%5B%22%E2%80%A2%22%2C1%2C%22Z%22%5D%2C%5B%22X%22%2C%22X%22%2C%22X%22%5D%2C%5B%22H%22%2C%22H%22%2C%22H%22%5D%5D%2C%22init%22%3A%5B0%2C0%2C0%2C0%2C0%2C0%2C0%2C0%2C1%2C0%2C1%2C%22-%22%5D%7D)
## **Components of the Circuit**
### **Loading of Data**
`Designing unitary operation to load all information of vector into quantum registers of quantum CPU from classical memory is called quantum loading scheme (QLS).`
* First, I have provided an implementation of quantumly accessible classical memory and its loading scheme.
* Querying is done using bit-value encoding : |a>|0> -->|a>|d_a>
* Using quantum index helps in loading the data of several indices using superposition.
* Uses qubits of the order `O(log N)`and has time complexity `O(N)`.

### **Modified Oracle**

* To compare the `d` and `s` registers and mark the states which we are searching.   
* Even though `s` register consists of only one of the states of the 2 states we are searching, it is also able to mark the other state. I have exploited their unique relation to use appropiate gates.
* Acts as a modified oracle for Grover's algorithm.

### **Grover's Diffuser**
Circuit that helps in amplitude amplification in Grover's algorithm.\
Operator is implementing (2|s><s|-I) for grover's algorithm.

### **Reset(Optional)**
* Reset the s-register to |0> state using unitary operators.
* To note that this is not required here as the |s> register remains unchanged and unentagled with other registers.
* Done here to visually show the state of the address register easily.

## **Final Circuit and Results**

* I have taken an additional qubit to make sure that Grover's algorithm doesn't encounter more marked states than unmarked states, therefore we are getting an additional qubit |0⟩ in the   beginning in bloch sphere
* But qubit-0 will always remain unentagled with the other address qubits as it is always 0 while acting as a control while data loading.
* In accordance to Grover's, we should iterate sqrt(n/#marked) times. If #marked values is not known apriori, we can find it using the QPE(Quantum Phase Estimation) algorithm.
* I have represented the final data and results in form of statevector of address register and its blochsphere.
* **Also, got positive results for arbitrary value of N and marked states.**

### **Given any vector of length N and no.of marked states or states we want to search, this circuit finds an equal superposition of its indices.**

## **Note**
* Reseting of the |s> register is not needed. Done here to visually show the state of the address register easily.
* The state-vector is converted to big-endian convection consistent with the bloch sphere result.
* I have obtained the final statevector of dimensions 2^n by considering the first 2^n entries. This won't give a wrong answer because I had deliberately reset all the qubits     except the address register for this purpose only, though we shouldn't do this in general.
* As qiskit is little-endian I have reversed the statevector entries using `.reverse_qargs()` method. All the results shown are hence big-endian.

## **Acknowledgements**
I collaborated during this task with one of my friends `Asish Kumar Mandoi` who is also my team member in QOSF Mentorship program. I am grateful of his new ideas, knowledge and motivation and his part in the successful completion of the task.

# **REFERENCES**
* [1] Michael A. Nielsen, Isaac L. Chuang - `(Section 6.5)Quantum Computation and Quantum Information`
* [2] John A. Cortese, Timothy M. Braje - `Loading Classical Data into a Quantum Computer` - [[https://arxiv.org/pdf/1803.01958.pdf]](https://arxiv.org/pdf/1803.01958.pdf)
* [3] Vittorio Giovannetti, Seth Lloyd, Lorenzo Maccone - `Quantum random access memory` - [[https://arxiv.org/pdf/0708.1879.pdf]](https://arxiv.org/pdf/0708.1879.pdf)
* [4] Pang Chao-Yang - `Loading N-Dimensional Vector into Quantum Registers from Classical Memory with O(logN) Steps` - [[https://arxiv.org/pdf/quant-ph/0612061.pdf]](https://arxiv.org/pdf/quant-ph/0612061.pdf)
* [5] Qiskit - [[https://qiskit.org/documentation/]](https://qiskit.org/documentation/)






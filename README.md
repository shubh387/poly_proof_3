# ZK CIRCUIT IMPLEMENTATION:

Creating a circuit using the circom programming language that implements the following logical gate:
![image](https://github.com/khushisnha/POLY-PROOF/assets/137313256/ab3a40b9-4900-4520-8d9c-a10934aaed26)

## Description

This code is written in the CIRCOM language, a domain-specific language (DSL) for writing arithmetic circuits. It defines a circuit template for checking whether c is the product of a and b. 

## Getting Started

### Executing program

To run this program, I have used online Gitpod (VS code). You can visit the Gitpod website at https://remix.ethereum.org/ .
Extension used for creating a new file is .circom , example: fileName.circom

```circom

pragma circom 2.0.0;

```

This line specifies the version of the CIRCOM language used for this code.

```circom

template GivenCircuit()

```

This defines a circuit template named "GivenCircuit." Templates are reusable circuit components that can be instantiated with specific input values.

```circom
   
// Signal Inputs
   signal input a;
   signal input b;  

```

Signal Inputs: a and b are input signals, and they will be provided externally when the circuit is run.

```circom

 // Signal from gates
   signal X;
   signal Y;

 // Final Signal output
   signal output Q;

```

Signal from Gates: X and Y are intermediary signals used in the circuit.

Final Signal Output: Q is the output signal of the circuit, which represents whether c is the product of a and b.

```circom

 // Component gates used to create custom curcuit
   component andGate = AND();
   component notGate = NOT();
   component orGate = OR();

```

Component Gates: The circuit uses three component templates: andGate, notGate, and orGate. These are reusable building blocks of the circuit, implemented as separate templates.

```circom

 // Circuit Logic
   andGate.a <== a;
   andGate.b <== b;
   X <== andGate.out;

   notGate.in <== b;
   Y <== notGate.out;

   orGate.a <== X;
   orGate.b <== Y;
   Q <== orGate.out;

```

Circuit Logic: The circuit is constructed using gates and logic operations.

andGate computes the logical AND operation between a and b and stores the result in X.
notGate computes the logical NOT operation on b and stores the result in Y.
orGate computes the logical OR operation between X and Y and stores the result in Q.

```circom

template AND() {
    signal input a;
    signal input b;
    signal output out;

    out <== a*b;
}

```

The AND() template is a CIRCOM component that performs a logical AND operation between two input signals a and b. It outputs the result of a AND b in the out signal.

```circom

 template NOT() {
    signal input in;
    signal output out;

    out <== 1 + in - 2*in;
}

```

The NOT() template is a CIRCOM component that computes the logical NOT operation on an input signal in and outputs the negation in the out signal. The operation can be described as out = NOT(in), where out is the negation of in.

```circom

 template OR() {
    signal input a;
    signal input b;
    signal output out;

    out <== a + b - a*b;
}

```

The OR() template is a CIRCOM component that computes the logical OR operation on input signals a and b, and outputs the result in the out signal. The operation can be described as out = a OR b.

```circom

 component main = GivenCircuit();

```

The code component main = GivenCircuit(); creates a CIRCOM component named main, which corresponds to the entire circuit defined in the GivenCircuit template. The main component checks whether the signal c is the multiplication of signals a and b based on the logic defined in the GivenCircuit template.

### Install
`npm i`

### Compile
`npx hardhat compile` 
This will generate the **out** file with circuit intermediaries and geneate the **MultiplierVerifier.sol** contract

### Prove and Deploy
`npx hardhat run scripts/deploy.ts`
This script does 4 things  
1. Deploys the MultiplierVerifier.sol contract
2. Generates a proof from circuit intermediaries with `generateProof()`
3. Generates calldata with `generateCallData()`
4. Calls `verifyProof()` on the verifier contract with calldata

With two commands you can compile a ZKP, generate a proof, deploy a verifier, and verify the proof 🎉

## Result:

![Screenshot 2024-07-12 150447](https://github.com/user-attachments/assets/b24edbf5-eaa3-450b-8453-20a419f7dbb5)

![Screenshot 2024-07-12 150426](https://github.com/user-attachments/assets/db56a65c-7320-4f27-bed4-9d4144f82589)


## Authors

Shubham kumar

## License

This project is licensed under the MIT License - see the License.md file for details.

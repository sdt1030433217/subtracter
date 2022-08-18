install circom and snarkjs
	npm install -g circom
	npm install -g snarkjs
	
create a circuit	
	circuit.circom
		
		template Subtracter() {
			signal private input a;
			signal private input b;
			signal output c;
			c <== a-b;
		}

		component main = Subtracter();


circom factor/circuit.circom -o circuit.json
	This will output circuit.json

snarkjs setup --circuit=circuit.json
	The setup will generate a proving key and a verification key, proving_key.json and verification_key.json respectively.


Calculate a witness:
	input.json
		{
			"a": 100,
			"b": 10
		}
	
calculate the witness :
snarkjs calculatewitness --circuit=circuit.json --input=input.json
	This generates witness.json with all the signals.



Generate proof:
snarkjs proof --witness=witness.json --provingkey=proving_key.json
	This generates proof.json and public.json which is the values of the public inputs and outputs. In this case it's just the output because our inputs were private.



Verify proof:
snarkjs verify --proof=proof.json --verificationkey=verification_key.json --public=public.json
	This will output OK if correct or INVALID if incorrect.


Smart contract to verify proofs:
snarkjs generateverifier --verificationkey=verification_key.json
	This outputs verifier.sol

Snarkjs conveniently provides a method to generate the transaction input parameters.
	snarkjs generatecall --proof=proof.json --public=public.json
	
	
	
The output will look similar to this:
		
		
		
		
		
last,Compiling verifier contract and Deploying verifier contract,Verify proofs using the deployed contract	
	
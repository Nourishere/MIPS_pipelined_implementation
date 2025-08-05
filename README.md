## Brief
This is a simple pipelined processor implementation for a subset of the MIPS core instructions [^1] using [Logisim](https://github.com/logisim-evolution/logisim-evolution.git). It supports hazard detection and static branch prediction.
## Instructions supported

| Name | Type       | Function               | Example              |
| ---- | ---------- | ---------------------- | -------------------- |
| add  | Arithmetic | Addition               | add $ t1,$ t2,$ t3   |
| sub  | Arithmetic | Subtraction            | sub $ t1,$ t2,$      |
| or   | Arithmetic | Logical OR             | or $ t1,$ t2,$       |
| and  | Arithmetic | Logical AND            | and $ t1,$ t2,$      |
| slt  | Arithmetic | Set on less than       | slt $ t1, $t2. $t3   |
| lw   | Memory     | Load word from memory  | lw $ t1, 4($ zero)   |
| sw   | Memory     | Store word into memroy | sw $ t1, 4($ zero)   |
| beq  | Branch     | Branch if equal        | beq $ t1,$ t2, label |
| j    | Branch     | Jump                   | j label              |
## The pipeline
The pipeline is composed of **five** pipeline stages, which is consistent with the classic MIPS design.
IF -> ID -> EX -> MEM -> WB
Where:
* IF: Instruction fetch
* ID: Instruction Decode
* EX: Execute
* MEM: Memory
* WB: Write back

The pipeline registers are then named: **ID/IF**, **IF/EX**, **EX/MEM**, and **MEM/WB** sequentially.
## Hazard detection and forwarding
Pipelining results in hazards [^2]which need to be addressed via forwarding or stalling.
The EX pipeline stage is equipped with a forwarding unit for data hazards.
The ID stage is equipped with a hazard detection unit that results in necessary stalls. Also, a branch forwarding unit is found.
## Branches
In order to reduce the overhead associated with branch instructions, the comparison and equality-checking processes are moved up to the ID stage. To account for this, a forwarding unit is added in the ID stage. 
A static branch-not-taken scheme is used; the branch address is set to the next chronological instruction. If the prediction is false, this next instruction is flushed. 
## Limitations
Below are possible limitations in this project.
1. Non-optimal branch-prediction scheme (Dynamic prediction can reduce time penalty even more.)
2. Limited to core instructions.
3. No support for routines.
4. No support for exceptions.
## References 
* This project was made possible because of the fine work by David A. Patterson and John L. Hennessy in their book **"Computer Organization and Design: The Hardware/Software Interface", Fifth Edition.** Most of the design philosophies are based on ideas mentioned throughout the book. I highly recommend going through it.
* I included some files known by the software name "Logic Friday" which I used in the design process of some blocks.
* I highly recommend going over my friend Abdulrahman and his collegues' work on a similar yet more advanced version of this project. You can find it [here](https://www.linkedin.com/feed/update/urn:li:activity:7328905675919540225?updateEntityUrn=urn%3Ali%3Afs_updateV2%3A%28urn%3Ali%3Aactivity%3A7328905675919540225%2CFEED_DETAIL%2CEMPTY%2CDEFAULT%2Cfalse%29).
* A more detailed documentation is available in this repository.
## License
Pull requests are welcome. 
[GPL](https://choosealicense.com/licenses/gpl-3.0/)


[^1]: This refers to a subset of the MIPS-32 instruction set that does core arithmetic operations. This does not include floating-point operations.
[^2]: Hazards are situations caused by unmet dependencies where a pipeline stage hasn't yet finished execution and a later stage needs that value. This is an example of data hazards. Other types are control and load-data hazards.

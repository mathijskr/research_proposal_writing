# Spectrebuster: proofing leakage-free speculative execution

Modern day CPUs start executing instructions in the future before the current instruction has completed in what is called a pipeline.
When the CPU encounters a branch instruction, it speculates on which instruction to execute next.
When it predicted the wrong instruction, the mistake is reverted.
However, some changes to the micro-architectural state are not reverted and can be observed by an adversary.
An example of micro-architectural state that is not always reverted is data loaded in the cache.
An adversary can time accesses to the cache to learn that a memory address was accessed during a misprediction.
The leakage of such information can be formally specified in a leakage contract [1].
We should not strive to never leak information under speculation because that would kill performance.
Instead, we must be certain that the software that we run on a specific CPU leaks no information that can be exploited by an adversary.
Currently, we lack a method of combining a leakage contract with software instructions to reason about the safety of software on a specific CPU.
I propose to develop such a method.
With Spectrebuster we can prove that a sequence of instructions does not leak information under speculation on for instance a newest generation Intel CPU, while it does leak on an older AMD CPU.
If we are able to prove that a sequence is safe on a CPU, we do not need to insert costly lfence or cpuid instructions that mitigate Spectre.
Spectrebuster will also be useful in finding Spectre gadgets in existing software.


[1] https://spectector.github.io/papers/hwsw-contracts.pdf

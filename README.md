Download Link: https://assignmentchef.com/product/solved-embsys105-assignment-3-port-ucos-to-the-nucleo-f401re-board
<br>
The goal of this assignment is to complete the port of uCOS to the NUCLEO board. Much of the port is already complete (the easy part!). Your job is to implement the assembly language code for critical section entry and exit, and context switch.

<ol>

 <li>Download and unzip the uCOSPortHW project contained in the zip file: uCOSPortHW.zip</li>

 <li>Open the uCOSPortHW.eww workspace in the EWARM IDE.</li>

 <li>Make sure the uCOSPortHW project builds.</li>

 <li>Launch TeraTerm (Baud rate: 38400)</li>

 <li>Build and upload the project and start it running.</li>

 <li>Notice that it prints some initial messages on the UART but doesn’t get very far before hanging.</li>

 <li>Break into the program.</li>

 <li>Notice it is stuck in a tight loop at the label OSStartHang in the file os_cpu_a.asm.</li>

 <li>Your job is to read the explanatory comments and add missing code to file os_cpu_a.asm to achieve the following:

  <ol>

   <li><strong>Implement OS_CPU_SR_Save()</strong>. This function gets called throughout uCOS when entering a critical section. It should capture the current interrupt enable/disable status given by register PRIMASK, then disable interrupts, then return the captured status as the function’s return value.</li>

   <li><strong>Implement OS_CPU_SR_Restore(). </strong>This function gets called throughout uCOS when exiting a critical section. It takes an argument consisting of the interrupt enable/disable status and copies it to the PRIMASK register, then returns.</li>

   <li><strong>Implement ContextSwitch().</strong> This code handles the PendSV exception. PendSV is only triggered by software in uCOS and only executes when no other interrupt is active. UCOS triggers PendSV by calling either OSCtxSw() or OSIntCtxSw() if it determines that a context switch is necessary.</li>

  </ol></li>

 <li>Clean your completed project by removing the Debug folder and zipping it into a file named uCOSPortHW_&lt;YourUwNetId&gt;.zip e.g. uCOSPortHW_janedoe.zip</li>

 <li>Submit your zip file by the due date.</li>

</ol>

<strong>Hints</strong>

<ul>

 <li>See Labrosse chapter 13 for the steps of OSCtxSw(). Note that on Cortex-M4 OSCtxSw() just triggers PendSV and the actual work of OSCtxSw() is done by your ContextSwitch.</li>

 <li>OSTCBCur-&gt;OSTCBStkPtr

  <ul>

   <li>OSTBCur is a global pointer to the task block of the current task.</li>

   <li>OSTCBStkPtr is the first field of the task control block struct</li>

  </ul></li>

 <li>uCOS priorities are 1 byte so use appropriate assembly language data movement instructions, i.e. LDRB/STRB.</li>

 <li>In uCOS, priorities are unique. No two tasks can have the same priority therefore the priority of a task also doubles as its task ID.</li>

 <li>This uCOS port uses only the Main stack pointer (MSP) throughout. However the Process stack pointer (PSP) is used tangentially as a flag to indicate whether the initial context switch has occurred: 4 means no, 0 means yes.</li>

 <li>Your uDebugger diagnostic Hard Fault handler may be useful for debugging; just remove the portion that hacks into the stack and replace it by a spin loop instead of returning from the handler.</li>

</ul>
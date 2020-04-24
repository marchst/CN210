# CN210 FUNDAMENTAL OF COMPUTER ARCHITECTURE
## วพ.210 สถาปัตยกรรมคอมพิวเตอร์
**เนื้อหารายวิชา**

**Computer** ประกอบด้วย

1.Central Processing Unit (CPU) เป็นหน่วยประมวลผลกลางซึ่งประกอบด้วย 

  * Artimetric and Logic Unit (ALU) ตัวคำนวนทางคณิตศาสตร์และตรรกศาสตร์  
  
  * Register เก็บข้อมูลระหว่างคำนวน
  
  * Control Unit หน่วยควบคุม
  
2.Main Memory หน่วยความจำหลัก

3.Input/Output ตัวรับค่าหรือแสดงผล

**MIPS** เป็น CPU ที่เลือกมาศึกษาในรายวิชา CN210 ซึ่งแต่ละคำสั่ง 32-bit เท่ากันหมด

### ** CLIP การบ้าน**

* [CLIP 1](https://youtu.be/-u3ELDG5a7I) **R-Format**

R- format เป็นคำสั่งประเภทหนึ่งของ MIPS ซึ่งมี 32-bit มี 6 ส่วน

|op | rs | rt | rd | shamt | func |
|---|---|---|---|---|---|
|6-bit|5-bit|5-bit|5-bit|5-bit|6-bit|

   
*func $rd, $rs, $rt*
   
ทำหน้าที่คำนวณข้อมูลที่เก็บไว้ที่ register rs และ register rt แล้วนำผลลัพธ์ที่ได้จากการคำนวณไปเก็บไว้ที่ register rd โดย func เป็นตัวกำหนดการคำนวณ ( +, - ,...) ในส่วนของ op ในคำสั่งประเภท R-Format จะมีค่าเป็น 0 ทั้ง 6-bit


* [CLIP 2](https://youtu.be/NWCN2Y-DYX4) **CPU**

ยกตัวอย่างการทำงานของ MIPS CPU ตั้งแต่การเปิดเครื่องคอมพิวเตอร์ขึ้นมา โดยเริ่มจากการดูว่าตำแหน่งเริ่มต้นของ CPU ชี้ไปที่ Address ใด ซึ่งตำแหน่งเริ่มต้นอาจไม่ใช่ตำแหน่ง 0 เสมอไป จากนั้นให้ทำงานตามคำสั่งที่เก็บไว้ใน Address นั้นๆ 1 คำสั่งมี 4-bit ดังนั้นเมื่อทำงานจบ 1 คำสั่งแล้วก็จะไปทำคำสั่งที่ 4-bit ถัดไป ใน CLIP จะประกอบไปด้วยคำสั่งทั้ง 3 ประเภท ของ MIPS ได้แก่ R-Format, I-Format และJ-Format มีรายละเอียดดังต่อไปนี้ 

   1. R-Format มี 6 ส่วน *(อธิบายไปแล้วในหัวข้อ CLIP 1 ด้านบน)*
      
   2. I-Format มี 4 ส่วน ใช้ในการเคลื่อนย้ายข้อมูลที่เก็บไว้ใน register
   
|op | rs | rt | value or offset |
|---|---|---|---|
|6-bit|5-bit|5-bit|16-bit|

*lw $rt, offset($rs)* , *sw $rt, offset($rs)*

   3. J-Format มี 2 ส่วน ใช้ในการ jump จากตำแหน่ง Address ปัจจุบันไปยังอีกตำแหน่งหนึ่ง
   
|op | absolute address |
|---|---|
|6-bit|26-bit|

*j address*

* [CLIP 3](https://youtu.be/GuDT-ue4UV4) **Single cycle VS Multi-cycle**

**Single cycle** : วงจร digital ที่อ่านและทำงานตามคำสั่งจบได้ใน 1 cycle (cycle = clock ของคอมพิวเตอร์) หรือสรุปสั้นๆได้ว่า 1 คำสั่งจบได้ใน 1 cycle และไม่ว่าจะทำคำสั่งใดจะใช้เวลาเท่ากันทั้งหมดคือ 8ns มี ALU มากกว่า 1 memory แบ่งเป็น 2 ส่วน คือ instruction memory และ data memory
![image](https://i.stack.imgur.com/vCvw1.png)

**Multi-cycle**  : วงจร digital ที่อ่านและทำงานตามคำสั่งจบในหลาย cycle เวลาในการทำแต่ละคำสั่งจะไม่เท่ากัน มี ALU 1 ตัว  instruction memory และ data memory รวมเป็นอันเดียวกัน มี Instruction register และ A, B พักข้อมูลจาก register ก่อนเข้าไปคำนวณใน ALU
![image](https://i.imgur.com/mWXHWpT.png)

* [CLIP 4](https://youtu.be/UYCXfCff6dE) **lw in Multi-cycle**

lw เป็นคำสั่งประเภท I- format *(อธิบายไปแล้วในหัวข้อ CLIP 2 ด้านบน)*

ซึ่งใน muti-cycle คำสั่ง lw มีทั้งหมด 5 cycle ดังต่อไปนี้
```
   T1 - IR = Memory[PC]                     อ่านว่าปัจจุบัน PC ชี้ไปที่ Address ใดใน Memory แล้วนำไปเก็บไว้ที่ Instruction Register 
        PC = PC + 4                         นำ PC ปัจจุบันบวก 4 เพื่อเตรียมตัวทำคำสั่งถัดไป เพราะ 1 คำสั่งใช้ 4 bytes 
        
   T2 - A  = Reg[IR[25-21]]                 นำข้อมูลที่ register rs ไปเก็บไว้ที่ A
        B  = Reg[IR[20-16]]                 นำข้อมูลที่ register rt ไปเก็บไว้ที่ B
        ALUout = PC + (sign-extend(IR[15-0])<<2)  
        
   T3 - ALUout = A + sign-extend(IR[15-0])  นำค่าที่ A เก็บไว้มาบวกกับ offset แล้วเก็บผลลัพธ์ที่ได้ไว้ใน ALUout
   
   T4 - MDR = Memory[ALUout]                นำผลลัพธ์จาก ALUout มาเก็บไว้ที่ Memory data register
   
   T5 - Reg[IR[20-16]] = MDR                นำค่าที่เก็บไว้ที่ Memory data register มาเก็บใน register rt ซึ่งคือ B
   
```

* [CLIP 5](https://youtu.be/htB5g3B2tR0) **beq in Multi-cycle**

beq เป็นคำสั่งประเภท I- format เป็นคำสั่ง jump แบบมีเงื่อนไข โดยจะดูว่าข้อมูลที่ register rs และ register rt เท่ากันหรือไม่ หากเท่ากันจะทำการ jump ไปยังตำแหน่งถัดไป

*beq $rs, $rt,offset*

ซึ่งใน muti-cycle คำสั่ง beq มีทั้งหมด 3 cycle ดังต่อไปนี้
```
   T1 - IR = Memory[PC]                     อ่านว่าปัจจุบัน PC ชี้ไปที่ Address ใดใน Memory แล้วนำไปเก็บไว้ที่ Instruction Register 
        PC = PC + 4                         นำ PC ปัจจุบันบวก 4 เพื่อเตรียมตัวทำคำสั่งถัดไป เพราะ 1 คำสั่งใช้ 4 bytes 
        
   T2 - A  = Reg[IR[25-21]]                 นำข้อมูลที่ register rs ไปเก็บไว้ที่ A
        B  = Reg[IR[20-16]]                 นำข้อมูลที่ register rt ไปเก็บไว้ที่ B
        ALUout = PC + (sign-extend(IR[15-0])<<2)  
        
   T3 - if(A==B) then PC = ALUout           นำค่าที่ A และ B มาเปรียบเทียบกัน หากเท่ากันจะเก็บผลลัพธ์ที่ได้ไว้ใน ALUout ถ้าไม้จะข้ามไปทำคำสั่งถัดไปทันที
   
```

* [CLIP 6](https://youtu.be/SFvhhpdckLI) **control signal R-format**

อธิบายการทำงาน control signal ของคำสั่ง R-format ทั้ง 4 cycle ดังนี้ 

```
   T1 - MemRead = 1                   Memory มีการใช้งาน
        IorD = 1                      อ่านว่าปัจจุบัน PC ชี้ไปที่ Address ใดใน Memory
        IRWrite = 1                   นำMemory ที่ถูกชี้ไปเก็บไว้ที่ Instruction Register
        ALUSrcA = 0                   Mux เลือกค่าจาก 0 ซึ่งคือ PC
        ALUSrcB = 1                   Mux เลือกค่าจาก 1 ซึ่งคือ 4
        ALUOP = ADD                   ทำการคำนวณโดยการบวกค่า PC กับ 4
        PCWrite = 1, PCSource = 1     นำผลลัพธ์การคำนวณเขียนทับ PC ปัจจุบัน
   T2 - ALUSrcA = 0                   Mux เลือกค่าจาก 0 ซึ่งคือ PC
        ALUSrcB = 3                   Mux เลือกค่าจาก 3 ซึ่งคือ offset
        ALUOP = 0                     ทำการคำนวณโดยการบวกค่า PC กับ offset
   T3 - ALUSrcA = 1                   Mux เลือกค่าจาก 1 ซึ่งคือ register rs
        ALUSrcB = 0                   Mux เลือกค่าจาก 0 ซึ่งคือ register rt
        ALUOP = 2                     ทำการคำนวณโดย func เป็นตัวกำหนดว่าจะคำนวณโดยวิธีใด
   T4 - RegWrite = 1                  นำ ALUout มาเขียนใน register rd
        MemtoReg = 0                  Mux เลือกค่าจาก 0 ซึ่งคือ ALUout
        RegDst = 1                    Mux เลือกค่าจาก 1 ซึ่งคือ register rd
   
```
* [CLIP 7](---) **Pipelining**

Pipelining คือ หน่วยความจำที่อยู่ระหว่าง CPU และ Memory ทำหน้าที่เก็บคำสั่ง และลำดับคำสั่งโดยการนำคำสั่งที่ต้องทำงานถัดไปมาต่อรอคำสั่งที่กำลังทำงานอยู่ปัจจุบันไม่ให้ CPU ว่าง สามารถทำคำสั่งถัดไปได้เลยโดยที่คำสั่งแรกยังไม่เสร็จ ทำให้ทำงานได้**เร็วขึ้น** 

เปรียบเทียบให้เห็นภาพโดยให้ 1 คำสั่ง คือ ผ้า 1 ถุง โดยในแต่ละคำสั่งจะมีขั้นตอนการทำงาน หรือการจัดการผ้าแต่ละถุงจะมีขั้นตอนวิธีการแต่ละขั้น ยกตัวอย่างการเปรียบเทียบการทำงานของ Pipelining ดังภาพต่อไปนี้

![image](http://3.bp.blogspot.com/-6RQaYhlYk2k/UKTYQVX9csI/AAAAAAAAAGQ/0xF1OxF_N_Y/s1600/02-What-is-pipelining-01.png)

![image](http://2.bp.blogspot.com/-4YXOlZ30iCQ/UKTYR4Y4FLI/AAAAAAAAAGk/pCdSkaaazVA/s1600/02-What-is-pipelining-02.png)

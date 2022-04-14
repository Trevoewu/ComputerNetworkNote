# Byte Oriented Approach


## Binary Synchronous Communication Protocol(BSC or BISYNC)

1. BYTE ORIENTED APPROACH  
    *it simply views the frame as collection of bytes or characters.*  

2. Byte Oriented Protocols  
   **BISYNC<->Binary Synchronous Communication Protocol**  
   PPP<->Point-to-Point Protocol
   DDCMP<->Digital Data Communication Message Protocol

3. BISYNC
    - It is a sentinel approach.  
    - Developed by IBM.  
    - Also preferred by BSC  
    - It is a byte-oriented protocol  

   *BI means two, SYN means the special starting character is SYN and C stands for communication*

   ![](2022-04-07-21-41-18.png)  
4. BISYNC - FRAME FORMAT  
   Frames transmitted beginning with left field  
   - Beginning of a frame is denoted by sending a special SYN(synchronize)character  
   - Data portion of the frame is contained between special sentinel character STX(start of text) and EXT(end of text).  
   *sentinel means it is going to to guard something. So the body is going to be guarded by this STX and ETX field*
   - SOH:Start of Header.  
   - DLE:Data Link Escape.  
   - CRC:Cyclic Redundancy Check.  

5. Character Stuffing  
    Byte stuffing or Character Stuffing is the process of adding one extra byte whenever there is a flag or escape character in the text.  
     *in this flag:SYN, STX, ETX ect*

     This is done by DLE in BISYNC protocol.  

## Point-to-Point Protocol(PPP)

1. PPP
   - PPP is a data link layer protocol  
   - PPP is a WAN protocol and which is commonly run over Internet links.  
   - It is widely used in broadband communications having heavy loads and high speeds  
   - It is used to transmit multiprotocol data between two directly connected(point-to-point) computers.(to be precise, point-to-point devices)  

1. PPP-Frame Format  
   ![](2022-04-07-22-34-55.png)  

   - Flag : 1 byte that marks the beginning and the end of the frame. the bit pattern of the flag is 01111110.  
   - Address : 1 byte set which is set to 11111111 in case of broadcast.  
   - Control : 1 byte set to a constant value of 11000000.  
   - Protocol : 1 or 2 bytes that define the type of data contained in the payload field.  
   - Payload : This carries the data from the network layer. The maximum length of the payload field is 1500 bytes. However, this may be negotiated between the endpoints of communication.  
   - Checksum : Error detection.  
  
## Digital Data Communication Message Protocol(DDCMP)

1. DDCMP  
   - Byte-oriented communication protocol.  
   - Devised by Digital Equipment Corporation.  
   - It is a **byte-counting** approach.  
   - Count field in the frame format.  
   - **Count**: How many bytes are contained in the frame body?  
  
1. DDCMP-Frame Format  


   ![](2022-04-07-23-00-06.png)  

1. Danger With The Count Field  


   One danger with this approach is that if transmission error could corrupt the count field then the end of the frame would not be correctly detected by the receiver.  
   ![](2022-04-07-23-06-32.png)

## Error Detection

1. Error 

   - Data are transmitted in network.  
   - The data can be corrupted during transmission.  
   - Transmission error.  
   - For reliable communication, errors must be detected and corrected.  
   -  Error detection and correction are implemented either at the data link layer or the transport layer of the OSI model.  
  
2. Type of Error

- Bit Error  
  - a.k.a single bit error.  
  - In a single bit error, only 1 bit in the data unit has been changed.  
  
  ![](2022-04-07-23-20-14.png)
- Burst error.  
  - In burst error, 2 or more bits in the data unit have changed.  

![](2022-04-07-23-22-05.png)

3. How to detected the error?

- Error detection means to decide whether the received data is correct or not without having a copy of the original message.  
- To detect or correct error, we need to send some extra bits with the data.  
- The extra bits are called as redundant bits.

![](2022-04-07-23-28-46.png)

4. Error Correction 

It can be handle in two ways:
  - Receiver can have the sender retransmit the entire data unit.  
  - The receiver can use an error-correcting code, which automatically corrects certain errors.

5. Error Detection/Correction

![](2022-04-07-23-36-02.png)

6. Error Detection Techniques

- Vertical Redundancy Check(VRC)  
- Longitudinal Redundancy Check(LRC)  
- Checksum  
- Cyclic Redundancy Check(CRC)

## Vertical Redundancy Check(VRC)

1. VRC

- Vertical Redundancy check.  
- A.k.a parity check  
- VRC = 1  if odd number of 1's  
- VRC = 0 if even number of 1's

![](2022-04-08-20-49-27.png)

2. Performance of VRC

- It can detect single bit error.  
- It can detect burst error only if the number of error is odd.  

- Sender:11100001 -> Transmission Error 1**0**100001 -> Receiver rejects this data.  
- Sender:11100001 -> Transmission Error 1**0**100**1**01 -> Receiver accept this data.  

## Longitudinal Redundancy Check(LRC)

- Longitudinal Redundancy Check.  
- In LRC, a block of bits is organized in rows and columns.  
- a.k.a Two Dimensional parity.  
- The parity bit is calculated for each column and sent along with the data.  
- The block of parity acts as the redundant bits.  

1. LRC-EXAMPLE

Find the LRC for the data blocks 11100111 11011101 00111001 10101001 and determine the data that is transmitted?

![](2022-04-08-21-12-02.png)  
![](2022-04-08-21-13-14.png)  

2. Performance of LRC

- LCR increases the likelihood of detecting burst error.  
- If two bits in one data units are damaged and two bits in exactly the same positions in another data unit are also damaged, the LRC checker will not detect an error.  

## Checksum 

**Checksum = Check + sum**  
Sender side - Checksum Creation.  
Receiver side - Checksum Validation  

1. Checksum - Operation at sender side

- Break the original message into 'k' number of of blocks with 'n' bits in each block.  
- Sum all the 'k' data blocks.  
- Add the carry(进位) to the sum, if any.  
- Do 1's complement(补码) to the sum = Checksum.  

![](2022-04-08-21-44-07.png)  
![](2022-04-08-21-44-51.png)

2. Checksum - operation at receiver side

- Collect all the data blocks including the checksum.  
- Sum all the data blocks and checksum.  
- If the result is all 1's, Accept; Else Reject.  

![](2022-04-08-21-50-00.png)  

3. Performance of Checksum

- The checksum detects all errors involving an odd number of bits.  
- It detects most errors involving an even number of bits.  
- if one or more bits of a segment are damaged and corresponding bit or bits of opposite value in a second segment are also damaged, the sums of those columns will not change and the receiver will not detect the error.

## Cyclic Redundancy Check(CRC) 

1. CRC Generation at sender side  
- Find the length of the divisor 'L'.  *除数*
- Append 'L-1' bits to the original message.*0's are appended.*    
- Perform binary division operation.*二进制除法*  
- Remainder(余数) of the division  = CRC.  
  **NOTE**:  
  The CRC must be of L-1 bits.  

2. CRC - operation st sender side    

L = 4; So, 3 0's are appended to the message.  
Binary division operation involves exclusive or operation(异或).  
Using polynomial long division.(多项式长除法)  

![](2022-04-08-23-37-46.png)  

CRC = 0 0 1  
Data Transmitted : 100100 **001**  

3. Homework 

- Find the CRC for 1110010101 with the divisor X<sup>3</sup> + X<sup>2</sup> + 1 ?  
- Suppose we want to transmit the message 11001001 and protect it from error using the CRC polynomial X<spu>3</sup>+1. Use polynomial long division to determine the message that should be transmitted. Corrupt the left-most third bit of the transmitted message and show that the error id detected by the receiver using CRC technique.  


![](2022-04-08-23-46-36.png)  


1. CRC - operation at receiver side  
If the remainder of the division performed by the receiver is 0, receiver will accept.  

![](2022-04-08-23-54-43.png)  




















  
  
    
   

INCLUDE Irvine32.inc
.model flat
.stack 2048
.386
.data

heading BYTE "BMI AND GRADE CALCULATOR",0
heading1 BYTE "LOGIN",0
username BYTE "Enter Your Username: ",0
password BYTE "Enter your password: ",0
loginfailed BYTE "Username or Password incorrect!. Do you want to try again? (Y-1/N-0): ",0
usercollection BYTE "shayan",0
pwcollection BYTE "pass",0
user BYTE 9 dup(?)         ;FOR USERNAME
pw BYTE 5 dup (?)         ;FOR PASSWORD
Again BYTE ?           ;For WHILE LOOP

msg1 byte "Enter obtained marks of Assembly Language : ",0 
msg2 byte "Your grade is : ",0
msg3 byte "Please Enter Marks between 0-99.",0

newLine db 13,10,'$'
countDigits dw 0
val dword 0
num1 dword 0
num2 dword 0

height real4 ?
weight real4 ?
result real4 0.0
X real4 100.0

inputMsg1 db "Enter your height in m: ",0
inputMsg2 db "Enter your weight in kg: ",0
outputMsg1 db "BMI : ",0
outputMsg2 db "BMI STATUS : ",0
Status1 db "You are Underweight.Increase your calorie intake!",0
Status2 db "Great. Your BMI is Normal!",0
Status3 db "You are Overweight!",0
Status4 db "You are Obese!",0
Status5 db "You are Extremely Obese!",0

bye byte "HOPE WE MEET AGAIN!",0
.code
main PROC
CLD
			;LOGIN
J1:
	call clrscr
	mov eax, white+(red*16)
	call SetTextcolor
	mov dl,45
	mov dx,45
	call gotoxy
	mov edx,offset heading
	call writestring
	mov ecx,5
	L1:
	mov al,'.'
	call writechar
	mov eax,1000
	call delay
	loop l1

	call crlf
	call crlf
	mov edx,offset heading1
	call writestring
	call crlf
	call crlf

	mov edx,offset username
	call writestring
	mov edx ,offset user
	mov ecx, lengthof user
	call readstring 

	mov edx,offset password
	call writestring
	mov edx, offset pw
	mov ecx, lengthof pw +1
	call readstring



mov esi , OFFSET usercollection
mov edi, OFFSET user
cmpsb
JE J2

mov edx, offset loginfailed
call writestring
call readint
mov Again,al
cmp Again,1
JE J1
JNE J5 

J2:
	mov esi, OFFSET pw
	mov edi, OFFSET pwcollection
	cmpsb
	JE J3

	mov edx,offset loginfailed
	call writestring
	call readint
	mov Again,al
	cmp Again,1
	JE J1
	JNE J5
	 
		;GRADE
J3:
	start:
		call crlf
		mov edx, offset msg1 
		call writestring
		call readint
		cmp eax,100
		jg invalid
		cmp eax,80
		jge gradeA
		cmp eax,70
		jge gradeB
		cmp eax,60
		jge gradeC
		cmp eax,50
		jge gradeD
		jmp gradeF
	gradeA:
		mov al, 'A'
		jmp gradeshow
	gradeB:
		mov al, 'A'
		jmp gradeshow
	gradeC:	
		mov al, 'A'
		jmp gradeshow
	gradeD:
		mov al, 'D'
		jmp gradeshow
	gradeF:
		mov al, 'F'
		jmp gradeshow
	gradeshow:
		mov edx, offset msg2
		call writestring
		call writechar
		call crlf
		jmp quit
	invalid:
		mov edx, offset msg3
		call writestring
		call crlf
		call crlf
		jmp start
	quit:
		call crlf

		;BMI
J4:

	;input height
	mov edx, offset inputMsg1 
	call writestring
	call readint
	mov height, eax

	;input weight
	mov edx, offset inputMsg2
	call writestring
	call readint
	mov weight, eax

	call findBMI

	call crlf
	call showBMIStatus
	
	J5:
	call crlf
	call crlf
	mov edx,offset bye
	call writestring
	call crlf
	call crlf
	call crlf


main endp
exit

findBMI proc
	pushad
	mov ebx,weight     ;w / h^2
	mov eax,height
	mov ecx,height
	mul ecx
	mov height,eax
	mov eax,weight
	mov ebx,height
	div ebx
	call crlf
	mov edx, offset outputMsg1
	call writestring

	call writeint
	mov weight,eax
	popad
	ret
findBMI endp

showBMIStatus proc
	cmp weight, 18
	JLE Underweight
	cmp weight, 25 
	JLE Normal
	cmp weight, 30
	JLE Overweight
	cmp weight, 35 
	JLE Obese
	cmp weight ,36
	JGE ExtremelyObese 
Underweight:
	mov edx, offset status1
	JMP exitShowBMIStatus
Normal:
	mov edx, offset status2
	JMP exitShowBMIStatus
Overweight:
	mov edx, offset status3 
	JMP exitShowBMIStatus
Obese:
	mov edx, offset status4 
	JMP exitShowBMIStatus
ExtremelyObese:
	mov edx, offset status5

exitShowBMIStatus:
	call writestring
	
	ret
showBMIStatus endp

end main
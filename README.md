# ASM-code
segment .data
	msg1 db "nhap so dau tien:"
	len1 equ $- msg1
	msg2 db "nhap so thu hai:"
	len2 equ $- msg2
	temp db "00000000000"
	msg3 db "tong cua hai so tren la:"
	len3 equ $- msg3

segment .bss
	num1 resb 11
	num1_rev resb 11
	num2_rev resb 11
	num2 resb 11
	total resb 11
segment .text
	global _start
_start:
	mov ecx,msg1					;in msg1
	mov edx,len1
	call print
	
	mov ecx,num1
	mov edx,11
	call scan
	
	mov esi,eax-1					; sau khi scan input vao thi thanh eax se luu do dai cua string vua nhap
	dec esi						; esi se la kieu nhu offset vi tri cua cac so va no sex chi ve so hang don vi
	mov edi,0
	mov ecx,eax-1					; ecx la counter de loop ham rev_num1 can loop no dung bang do dai cua str
	dec ecx
	clc
rev_num1:
	mov al,[num1+esi-1]				;luu gia tri cua tung chu so bat dau tu hang don vi vao thanh ghi al
	add al,"0"
	aaa						; adjust after addition	
	pushf
	or al,0x30
	popf
	
	mov [num1_rev+edi],al				; luu gia tri cua thanh ghi al vao num1_rev bat dau tu vi ti dau tien de dao nguoc chuoi
	dec esi
	inc edi
	loop rev_num1
	
	mov ecx,msg2					; in msg2
	mov edx,len2
	call print
	
	mov ecx,num2
	mov edx,11
	call scan
	
	mov esi,eax-1					;tu day thi cac buoc nhu tren
	dec esi
	mov edi,0
	mov ecx,eax-1
	dec ecx
	clc
rev_num2:
	mov al,[num2+esi-1]
	add al,"0"
	aaa
	pushf
	or al,0x30
	popf
	
	mov [num2_rev+edi],al
	dec esi
	inc edi
	loop rev_num2
	
	mov ecx,10
	mov esi,0
	mov edi,9
	clc
add_loop:
	mov al,[num1_rev+esi]				; sau khi dao nguoc chuoi nhap vao thi minh cong chuoi vo
	adc al,[num2_rev+esi]				; adc la addd carry neu nhu co phep du thi no se cong vo hang tiesp theo
	aaa
	pushf
	or al,0x30
	popf
	
	mov [total+edi],al				; cong xong thi can phari luu vao total bat dau tu vi tri cuoi cung
	dec edi
	inc esi	
	loop add_loop
	
	
	;print
	mov edx,10
	mov ecx,total
	
	mov edx,len3
	mov ecx,msg3
	call print
	mov edx,10
	mov ecx,total
	call print
	;exit
	mov eax,0x1
	int 0x80
	

print:
	mov eax,0x4
	mov ebx,0x1
	int 0x80
	ret

scan:
	mov eax,3
	mov ebx,0
	int 0x80
	ret

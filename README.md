*This project has been created as part of the 42 curriculum by gkim*

## Description
static variable 이해
file descriptor 이해 및 줄개념 구현

## Instructions
get_next_line(int fd)는 호출할 때마다 한 줄을 반환해야 함
반환되는 문자열에는 개행 문자(\n)가 포함될 수 있음
EOF 또는 오류 발생 시 NULL을 반환해야 함
read()와 BUFFER_SIZE를 사용하여 파일을 읽어야 함
함수는 여러 번 호출되어도 읽기 상태를 유지해야 함
반환된 문자열은 free() 가능해야 하며 메모리 누수가 없어야 함

## Resources

- fd관련
https://velog.io/@whwogur/%ED%8C%8C%EC%9D%BC-%EB%94%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%84%B0File-descriptor-%EC%86%8C%EC%BC%93-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D

- static variable 관련
https://blog.naver.com/dd1587/221106199316


## AI usage
자연스러운 번역, 오타검수
영어 원문 자료 번역

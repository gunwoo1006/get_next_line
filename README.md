*This project has been created as part of the 42 curriculum by gkim*

## Description
Get Next Line은 파일 디스크립터로부터 데이터를 한 줄씩 읽어오는 함수를 구현하는 프로젝트입니다.

이 프로젝트의 목표는 파일 디스크립터(File Descriptor), 동적 메모리 할당, 그리고 static 변수의 동작 원리를 이해하는 것입니다.

get_next_line() 함수는 호출될 때마다 파일 또는 표준 입력으로부터 한 줄을 읽어 반환하며, 줄 끝에 개행 문자(`\n`)가 존재하는 경우 이를 포함하여 반환합니다.

또한 이 프로젝트를 통해 버퍼 기반 입력 처리, 호출 간 데이터 보존, 그리고 다양한 BUFFER_SIZE 환경에서의 효율적인 파일 읽기 방법을 학습할 수 있습니다

## Instructions
- get_next_line.c

- CC : cc

- flag : -Wall -Wextra -Werror

### Make
- Build: `make`
- Clean object files: `make clean`
- Remove all generated files: `make fclean`
- Rebuild: `make re`

### Allowed Functions
- malloc
- free
- read

### Authorized Headers
- unistd.h
- stdlib.h

### Requirements
- get_next_line(int fd)는 호출할 때마다 한 줄을 반환해야 함
- 반환되는 문자열에는 개행 문자(\n)가 포함될 수 있음
- EOF 또는 오류 발생 시 NULL을 반환해야 함
- read()와 BUFFER_SIZE를 사용하여 파일을 읽어야 함
- 함수는 여러 번 호출되어도 읽기 상태를 유지해야 함
- 반환된 문자열은 free() 가능해야 하며 메모리 누수가 없어야 함

### Algorithm

get_next_line() 함수는 파일 디스크립터로부터 데이터를 읽어 정적(static) 변수에 저장하고, 개행 문자(`\n`)를 기준으로 한 줄씩 반환한다.

동작 과정은 다음과 같다.

1. `read()`를 사용하여 파일 디스크립터에서 데이터를 읽는다.
2. 읽어온 데이터를 static 변수(`storage`)에 누적 저장한다.
3. 저장된 데이터에 개행 문자(`\n`)가 포함될 때까지 읽기를 반복한다.
4. 개행 문자까지의 내용을 새로운 문자열로 추출하여 한 줄을 생성한다.
5. 생성된 한 줄을 반환한다.
6. 반환한 줄 이후의 남은 데이터는 static 변수에 보관한다.
7. 다음 호출 시 보관된 데이터부터 이어서 처리한다.
8. 더 이상 읽을 데이터가 없으면 남아 있는 내용을 마지막 줄로 반환하고, 이후에는 `NULL`을 반환한다.

이 알고리즘을 통해 함수 호출 간 읽기 상태를 유지할 수 있으며, 파일 전체를 한 번에 읽지 않고도 한 줄씩 효율적으로 처리할 수 있다.

## Data Structure

이 프로젝트에서는 복잡한 자료구조를 사용하지 않는다.

주요 구성 요소는 다음과 같다.

- Static Variable : 호출 간 읽기 상태 유지
- Dynamic Memory Allocation : 반환할 문자열 생성
- Character Array (String) : 읽은 데이터 저장 및 처리
- File Descriptor : 입력 대상 식별

특히 static 변수는 이전 호출에서 읽고 남은 데이터를 보관하여 다음 호출에서 이어서 사용할 수 있도록 한다.

## Usage Example

```c
int		fd;
char	*line;

fd = open("example.txt", O_RDONLY);
while ((line = get_next_line(fd)))
{
	printf("%s", line);
	free(line);
}
close(fd);
```

### test main
```c
#include <fcntl.h>
#include <stdio.h>
#include <stdlib.h>
#include "get_next_line.h"

int	main(int argc, char **argv)
{
	int		fd;
	char	*line;
	int		i;

	if (argc != 2)
	{
		printf("Usage: %s <file>\n", argv[0]);
		return (1);
	}
	fd = open(argv[1], O_RDONLY);
	if (fd < 0)
	{
		perror("open");
		return (1);
	}
	i = 1;
	while ((line = get_next_line(fd)))
	{
		printf("[%d] %s", i++, line);
		free(line);
	}
	close(fd);
	return (0);
}
```

```c
#include <fcntl.h>
#include <stdio.h>
#include <stdlib.h>
#include "get_next_line.h"

int	main(void)
{
	int		fd;
	char	*line;

	fd = open("test.txt", O_RDONLY);
	if (fd < 0)
		return (1);
	line = get_next_line(fd);
	if (line)
	{
		printf("%s", line);
		free(line);
	}
	close(fd);
	return (0);
}
```

```c
#include <fcntl.h>
#include <stdio.h>
#include <stdlib.h>
#include "get_next_line.h"

int	main(void)
{
	int		fd;
	char	*line;

	fd = open("test.txt", O_RDONLY);
	if (fd < 0)
		return (1);
	while (1)
	{
		getchar(); // 엔터 대기
		line = get_next_line(fd);
		if (!line)
			break ;
		printf("%s", line);
		free(line);
	}
	close(fd);
	return (0);
}
```

### compile
```c
cc -Wall -Wextra -Werror -D BUFFER_SIZE=42 \
get_next_line.c get_next_line_utils.c main.c
```

### memory leak check
```c
valgrind --leak-check=full \
--show-leak-kinds=all \
--track-origins=yes \
./a.out test.txt
```

## Project Structure

├── get_next_line.c
├── get_next_line_utils.c
├── get_next_line.h
└── README.md

## Edge Cases

The implementation correctly handles:

- Empty files
- Files ending without '\n'
- BUFFER_SIZE = 1
- Large BUFFER_SIZE values
- Standard input
- Invalid file descriptors

## Resources

- fd관련
https://velog.io/@whwogur/%ED%8C%8C%EC%9D%BC-%EB%94%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%84%B0File-descriptor-%EC%86%8C%EC%BC%93-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D

- static variable 관련
https://blog.naver.com/dd1587/221106199316


### AI usage
자연스러운 번역, 오타검수
영어 원문 자료 번역
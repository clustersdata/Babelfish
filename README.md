# Babelfish

Babelfish

Description

You have just moved from Waterloo to a big city. The people here speak an incomprehensible dialect of a foreign language. Fortunately, you have a dictionary to help you understand them.

Input

Input consists of up to 100,000 dictionary entries, followed by a blank line, followed by a message of up to 100,000 words. Each dictionary entry is a line containing an English word, followed by a space and a foreign language word. No foreign word appears more than once in the dictionary. The message is a sequence of words in the foreign language, one word on each line. Each word in the input is a sequence of at most 10 lowercase letters.

Output

Output is the message translated to English, one word per line. Foreign words not in the dictionary should be translated as "eh".

Sample Input

dog ogday

cat atcay

pig igpay

froot ootfray

loops oopslay

atcay

ittenkay

oopslay

Sample Output

cat

eh

loops

Hint

Huge input and output,scanf and printf are recommended.

题目大意就是给你字典的对应信息，然后给你查询条件要求你输出字典查询结果，如果字符串没有在字典中则输出"eh"。
恩，学到了一个新的函数bsearch()，可以进行二分查找。先对字典使用qsort排序然后再使用bsearch()查找字符串即可。
还学到了一个输入函数sscanf()，可以从一个字符串中读取内容，格式如下

sscanf (string str, string format [, string var1...])


#include <stdio.h>

#include <stdlib.h>

#include <string.h>

 
typedef struct

{

	char en[11];
  
	char fn[11];
  
}dict;

 
dict a[100001];
 
 
/* 定义qsort比较函数 */

int q_cmp(const void * a,const void *b)

{

    return strcmp(((dict*)a)->fn, ((dict*)b)->fn);
    
}

 
/* 定义bsearch比较函数 */

int b_cmp(const void* a, const void* b)

{

    return strcmp((char*)a, ((dict*)b)->fn);
    
}

 
int main()

{

	char str[30];
  
	int i, sign;
  
	dict *p;
  
 
	i = 0;
  
	/* 查询标记记为"未开始" */
  
	sign = 1;
  
	/* 读取字符串直到文件结束 */
  
	while(gets(str))
  
	{
  
		/* 遇到空行则开始排序字典 */
    
		if (str[0] == '\0')
    
		{
    
			/* 查询标记记为"开始" */
      
			sign = 0;
      
			qsort(a, i, sizeof(dict), q_cmp);
      
			continue;
      
		}
    
		/* 查询未开始时读取字典信息 */
    
		if (sign)
    
		{
    
			/* 使用sscanf从str中读取查询要求 */
      
			sscanf(str, "%s %s", a[i].en, a[i].fn);
      
			i++;
      
		}
    
		/* 查询开始时进行查询 */
    
		else
    
		{
    
			/* 二分查找指定字符串 */
      
			p = (dict*) bsearch(str, a, i, sizeof(dict), b_cmp);
      
			/* 找到则输出结果 */
      
			if (p)
      
			{
      
				puts(p->en);
        
			}
      
			/* 找不到输出"eh" */
      
			else
      
			{
      
				puts("eh");
        
			}
      
		}
    
	}
  
	//system("pause");
  
	return 0;
  
}


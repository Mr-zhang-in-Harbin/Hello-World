//this is a new version for exam

#include <stdio.h>
#include <conio.h>
#include <stdlib.h>
#include <time.h>
#include <windows.h>

int x,y,ix,iy;
int i=0;
int num[4][4];
int exame[4][4];/*锁定数组值避免多次累和*/ 
int option=0;/*检查是否存在有效操作决定是否刷出新的数字*/
int full;
int score=0;

void show ();/*显示*/ 
void play (); /*游戏主体，判断程序走向*/ 
void left ();/*左移算法*/ 
void right ();
void up ();
void down ();
void star ();/*初始化*/ 
void newnumber();/*空白处加入新数字*/ 
void exam();/*解除锁定，检查是否游戏结束*/ 


/*主函数开始*/
int main ()
{
	srand((unsigned)time(NULL));
	star ();
	show ();
	while (1)
	{
		system ("cls");
		show();
		play ();
		if (option==1)
		{
			newnumber();
		}
		option=0;
		exam();
		if (full==0)
		{
			printf("game over\n");
			system("pause");
		}
	}
	return 0;
}

/*主函数结束*/ 

void star ()
{
	y=rand()%4,x=rand()%4;
	num[x][y]=2;
}

void show ()
{
	printf("|        |        |        |        |\n");
	for (y=0;y<=3;y++)
	{
		for (x=0;x<=3;x++)
			{
				if (num[x][y]!=0)
				{
					if (num[x][y]>=10&&num[x][y]<100)
						printf("|   %d   ",num[x][y]);
					else if (num[x][y]<10)
						printf("|   %d    ",num[x][y]);
					else if (num[x][y]>=100)
						printf("|   %d  ",num[x][y]);
					else if (num[x][y]>=1000)
						printf("|   %d ",num[x][y]);
				}
				else printf("|        ");
			}
			printf("|\n|        |        |        |        |\n");
			printf("-------------------------------------");
			printf("\n");
			if (y!=3)
				printf("|        |        |        |        |\n");
	}
	printf("score=%d\n",score);
}

void play()
{
	int input;
	scanf("%d",&input);
	if (input==8)
		up (),up (),up (),up (),up (),up ();/*重复进行以保证多次位移*/
	else if (input==5)
		down (),down (),down (),down (),down (),down ();
	else if (input==4)
		left (),left (),left (),left (),left (),left ();
	else if (input==6)
		right (),right (),right (),right (),right (),right ();
}

void up()
{
	
	for (x=0;x<=3;x++)
	{
		for (y=0;y<=3;y++)
		{
			if (y>=1&&num[x][y]>=2)/*防止下标溢出和无效的对0的检查*/ 
			{
			if (num[x][y-1]==0)
				num[x][y-1]=num[x][y],num[x][y]=0,option=1;
			else if (num[x][y-1]==num[x][y]&&exame[x][y]==0)
				{
				num[x][y-1]+=num[x][y];
				score+=num[x][y];
				num[x][y]=0;
				exame[x][y-1]=1;
				option=1;
				}
			}
		}
	}
}

void down()
{
	for (x=0;x<=3;x++)
	{
		for (y=3;y>=0;y--)
		{
			if (y<=2&&num[x][y]>=2)
			{
			if (num[x][y+1]==0&&num[x][y]>0)
				{
				num[x][y+1]=num[x][y];
				num[x][y]=0;
				option=1;
				}
			else if (num[x][y+1]==num[x][y]&&exame[x][y]==0)
				{
				num[x][y+1]+=num[x][y];
				score+=num[x][y];
				num[x][y]=0;
				exame[x][y+1]=1;
				option=1;
				}
			}
		}
	}
}

void left()
{
	for (x=0;x<=3;x++)
	{
		for (y=0;y<=3;y++)
		{
			if (x>=1&&num[x][y]>=2)
			{
			if (num[x-1][y]==0&&num[x][y]>0)
				num[x-1][y]=num[x][y],num[x][y]=0,option=1;
			else if (num[x-1][y]==num[x][y]&&exame[x][y]==0)
				{
				num[x-1][y]+=num[x][y];
				score+=num[x][y];
				num[x][y]=0;
				exame[x-1][y]=1;
				option=1;
				}
			}
		}
	}
}

void right()
{
	for (x=3;x>=0;x--)
		{
			for (y=0;y<=3;y++)
			{
				if (x<=2&&num[x][y]>=2)
				{
				if (num[x+1][y]==0&&num[x][y]>0)
					num[x+1][y]=num[x][y],num[x][y]=0,option=1;
				else if (num[x+1][y]==num[x][y]&&exame[x][y]==0)
					{
					num[x+1][y]+=num[x][y];
					score+=num[x][y];
					num[x][y]=0;
					exame[x+1][y]=1;
					option=1;
					}
				}
			}
		}
}

void newnumber()
{
	for (i=0;i==0;)
	{
		y=rand()%4,x=rand()%4;
		if (num[x][y]==0)
			i=1;	
	}
	num[x][y]=2;
}

void exam()/*检查游戏是否结束的功能还没加*/ 
{
	full=0;
	for (y=0;y<=3;y++)
	{
		for (x=0;x<=3;x++)
			exame[x][y]=0;
	}
	for (y=0;y<=3;y++)
	{
		for (x=0;x<=3;x++)
		{
			if (num[x][y]==0)
				full++;
			}
	}
	if (full=0)
		printf("game over"),system("pause");
		
}

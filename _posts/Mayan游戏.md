---
layout: post
title: Maya游戏 # 标题 
category : Intro #  
tags : [intro, tag1, tag2]
---

## Demo Post

题目描述
Mayan puzzle是最近流行起来的一个游戏。游戏界面是一个 7 行5 列的棋盘，上面堆放着一些方块，方块不能悬空堆放，即方块必须放在最下面一行，或者放在其他方块之上。游戏通关是指在规定的步数内消除所有的方块，消除方块的规则如下：
1 、每步移动可以且仅可以沿横向（即向左或向右）拖动某一方块一格：当拖动这一方块时，如果拖动后到达的位置（以下称目标位置）也有方块，那么这两个方块将交换位置（参见输入输出样例说明中的图6 到图7 ）；如果目标位置上没有方块，那么被拖动的方块将从原来的竖列中抽出，并从目标位置上掉落（直到不悬空，参见下面图1 和图2）；
[图？2333] 
2 、任一时刻，如果在一横行或者竖列上有连续三个或者三个以上相同颜色的方块，则它们将立即被消除（参见图1 到图3）。

注意：
a) 如果同时有多组方块满足消除条件，几组方块会同时被消除（例如下面图4 ，三个颜色为1 的方块和三个颜色为 2 的方块会同时被消除，最后剩下一个颜色为 2 的方块）。
b) 当出现行和列都满足消除条件且行列共享某个方块时，行和列上满足消除条件的所有方块会被同时消除（例如下面图5 所示的情形，5 个方块会同时被消除）。
3 、方块消除之后，消除位置之上的方块将掉落，掉落后可能会引起新的方块消除。注意：掉落的过程中将不会有方块的消除。
上面图1 到图 3 给出了在棋盘上移动一块方块之后棋盘的变化。棋盘的左下角方块的坐标为（0, 0 ），将位于（3, 3 ）的方块向左移动之后，游戏界面从图 1 变成图 2 所示的状态，此时在一竖列上有连续三块颜色为4 的方块，满足消除条件，消除连续3 块颜色为4 的方块后，上方的颜色为3 的方块掉落，形成图 3 所示的局面。
输入输出格式
输入格式：
输入文件mayan.in，共 6 行。
第一行为一个正整数n ，表示要求游戏通关的步数。
接下来的5 行，描述 7*5 的游戏界面。每行若干个整数，每两个整数之间用一个空格隔开，每行以一个0 结束，自下向上表示每竖列方块的颜色编号（颜色不多于10种，从1 开始顺序编号，相同数字表示相同颜色）。
输入数据保证初始棋盘中没有可以消除的方块。
输出格式：
输出文件名为mayan.out。
如果有解决方案，输出 n 行，每行包含 3 个整数x，y，g ，表示一次移动，每两个整数之间用一个空格隔开，其中（x ，y）表示要移动的方块的坐标，g 表示移动的方向，1 表示向右移动，-1表示向左移动。注意：多组解时，按照 x 为第一关健字，y 为第二关健字，1优先于-1 ，给出一组字典序最小的解。游戏界面左下角的坐标为（0 ，0 ）。
如果没有解决方案，输出一行，包含一个整数-1。
搜索的好题....【...】
搜索，对于每个点来说，一个点左移可以看做另一个点右移，而那个点的x一定更小，即字典序更小
所以我们只考虑右移的情况就好了
左移出现在哪里呢？
一个空格右移，可以看做有颜色的格子左移（我们不能动空格）
这就是格子左移了
#include<iostream>
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
int gg[10][10],m,num[11],n,map[10][10],ans[100][4];
void down()
{
	for(int i=0;i<5;++i)
	{
		for(int j=1;j<=7;++j)
		{
			if(map[i][j])
			{
				int now=j;
				while(now>1&&!map[i][now-1])
				{
					swap(map[i][now],map[i][now-1]);
					now--;
				}
			}
		}
	}
}
bool boom()
{
	memset(gg,0,sizeof(gg));
	for(int i=0;i<5;++i)
	{
		for(int j=1;j<=7;++j)
		{
			if(!map[i][j]) continue;
			if(map[i][j]==map[i-1][j]&&map[i][j]==map[i+1][j])
			{
				gg[i][j]=gg[i-1][j]=gg[i+1][j]=1;
			}
			if(j-1>0&&j+1<=7&&map[i][j]==map[i][j-1]&&map[i][j]==map[i][j+1])
			{
				gg[i][j]=gg[i][j-1]=gg[i][j+1]=1;	
			}
		}
	}
	int fl=0;
	for(int i=0;i<5;++i)
	{
		for(int j=1;j<=7;++j)
		{
			if(gg[i][j]) map[i][j]=0,fl=1;
		}
	}
	return fl;
}
bool can()
{
	for(int i=0;i<5;++i)
	{
		for(int j=1;j<=7;++j)
		{
			if(map[i][j]) return false;
		}
	}
	return true;
}
void dfs(int x)
{
	memset(num,0,sizeof(num));
	int last[10][10];
	if(x==n+1)
	{
		if(can())
		{
			for(int i=1;i<=n;++i)
			{
				for(int j=1;j<=3;++j)
				{
					printf("%d ",ans[i][j]);
				}
				printf("\n");
			}
			exit(0);
		}
		return;
	}
	for(int i=0;i<5;++i)
	{
		for(int j=1;j<=7;++j)
		{
			last[i][j]=map[i][j];
			num[map[i][j]]++;
		}
	}
	for(int i=1;i<=m;++i)
	{
		if(num[i]&&num[i]<3) return;
	}
	for(int i=0;i<5;++i)
	{
		for(int j=1;j<=7;++j)
		{
			if(i<4&&map[i][j]^map[i+1][j])
			{
				if(!map[i][j]) ans[x][1]=i+1,ans[x][2]=j-1,ans[x][3]=-1;
				else ans[x][1]=i,ans[x][2]=j-1,ans[x][3]=1;
				swap(map[i][j],map[i+1][j]);
				down();while(boom()) down();
				down();while(boom()) down();
				dfs(x+1);
				for(int ii=0;ii<5;++ii)
				{
					for(int jj=1;jj<=7;++jj)
					{
						map[ii][jj]=last[ii][jj];
					}
				}
			}
		/*	if(i>0&&map[i][j]^map[i-1][j]&&map[i-1][j]==0)
			{
				swap(map[i][j],map[i-1][j]);
				down();while(boom()) down();
				down();while(boom()) down();
				ans[x][1]=i,ans[x][2]=j-1,ans[x][3]=-1;
				dfs(x+1);
				for(int ii=0;ii<5;++ii)
				{
					for(int jj=1;jj<=7;++jj)
					{
						map[ii][jj]=last[ii][jj];
					}
				}
			}*/
		}
	}
}
int main()
{
	scanf("%d",&n);
	for(int i=0;i<=4;++i)
	{
		int j=1,say;
		while(1)
		{
			scanf("%d",&say);
			if(say==0) break;
			map[i][j++]=say;
			m=max(m,say);
		}
	}
	dfs(1);
	printf("-1");
	return 0;
 } 

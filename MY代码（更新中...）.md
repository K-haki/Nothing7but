```c

\#include<iostream>

\#include<cstdio>

using namespace std;

void chansform(int c,char *a); //函数1：IP地址十进制表示转换成点分十进制表示(参数1：IP十进制 ， 参数2：存储转换后的点分十进制IP地址的字符数组地址)

int ip_check(char i[]); //函数2：检测IP地址是否匹配，并以0/1作为返回值表示否/是，返回-1表示没有数据读入，即所有规则匹配结束

int port_check(int p); //函数3：检测端口是否匹配，并以0/1作为返回值表示否/是，返回-1表示没有数据读入，即所有规则匹配结束

int tcp_check(int t); //函数4：检测传输协议是否匹配，并以0/1作为返回值表示否/是，返回-1表示没有数据读入，即所有规则匹配结束

int judge(int a, int b, int c, int d, int e); //遍历规则集，以五元组作为参数传入，调用函数2/3/4并判断其返回是否都为1，

​                //若是，则返回规则编号；若否，则返回-2，继续循环读入下一条rule，再次调用函数2/3/4继续判断；

​                //check_1的值为-1,表示此时编号已经为规则集内最大，则返回-1

int main(){

  freopen("packet1.txt", "r", stdin);

//   freopen("output.txt", "w", stdout);

  char ip_rule_1,ip_rule_2,cip_1[20],cip_2[20];//cip_1[]和cip_2[]用于存储转换后的IP地址

  int ip_1,ip_2,ans;

  int port_1,port_2,tcp,tcp_r,port_r1,port_r2;

  int check_1,check_2,check_3,check_4,check_5;

  while(scanf("%d %d %d %d %d",&ip_1,&ip_2,&port_1,&port_2,&tcp)==5){

​    int f=0;//  f标记是否遍历规则集仍无法匹配而直接进入下一条数据的匹配；

​    chansform(ip_1,cip_1);

​    chansform(ip_2,cip_2);

​    do{

​      check_1=ip_check(cip_1);

​      check_2=ip_check(cip_2);

​      check_3=port_check(port_1);

​      check_4=port_check(port_2);

​      check_5=tcp_check(tcp);

​      ans=judge(check_1,check_2,check_3,check_4,check_5);

​      if(ans==-1){

​        printf("匹配失败！\n");

​        f=1;

​        break;

​      }

​    }while(ans==-2);

​    if(f) continue;

​    else printf("%d",ans);

  }



  return 0;

}



int judge(int a, int b, int c, int d, int e){

  int cnt=0;       //cnt用于记录规则编号；

  if(a==-1) return -1;  //a的值为-1,表示此时编号已经为规则集内最大，则返回-1

  else if(a==b&&b==c&&c==d&&d==e&&a==1){

​    return cnt;

  }else{

​    cnt++;

​    return -2;

  }

}

```
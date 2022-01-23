```c++

#include<bits/stdc++.h>
using namespace std;
typedef long long int ll;
string ip_rule="";//用于存储转换二进制后的规则IP网络前缀
int ip_chansform(void);//转换规则IP，返回前缀长度
// void chansform(int c,char *a); //函数1：IP地址十进制表示转换成点分十进制表示(参数1：IP十进制 ， 参数2：存储转换后的点分十进制IP地址的字符数组地址)
int ip_check(char a[]); //函数2：检测IP地址是否匹配，并以0/1作为返回值表示否/是，返回-1表示没有数据读入，即所有规则匹配结束
int port_check(int p); //函数3：检测端口是否匹配，并以0/1作为返回值表示否/是，返回-1表示没有数据读入，即所有规则匹配结束
int tcp_check(int t); //函数4：检测传输协议是否匹配，并以0/1作为返回值表示否/是，返回-1表示没有数据读入，即所有规则匹配结束
int judge(int a, int b, int c, int d, int e); //遍历规则集，以五元组作为参数传入，调用函数2/3/4并判断其返回是否都为1，
                               //若是，则返回规则编号；若否，则返回-2，继续循环读入下一条rule，再次调用函数2/3/4继续判断；
                               //check_1的值为-1,表示此时编号已经为规则集内最大，则返回-1






/*int judge(int a, int b, int c, int d, int e){
    int cnt=0;             //cnt用于记录规则编号；
    if(a==-1) return -1;  //a的值为-1,表示此时编号已经为规则集内最大，则返回-1
    else if(a==b&&b==c&&c==d&&d==e&&a==1){
        return cnt;
    }else{
        cnt++;
        return -2;
    }
}*/

int main(){
    freopen("input.txt","r",stdin);
    freopen("out.txt","w",stdout);
    char cip_1[35],cip_2[35];//cip_1[]和cip_2[]用于存储转换后的IP地址
    ll ip_1,ip_2,ans,i;
    int port_1,port_2,tcp,tcp_r,port_r1,port_r2;
    int check_1,check_2,check_3,check_4,check_5;
    while(cin>>ip_1>>ip_2){
        int f=0,cnt=0;//  f标记是否遍历规则集仍无法匹配而直接进入下一条数据的匹配；
        ltoa(ip_1,cip_1,2);
        ltoa(ip_2,cip_2,2);
    //    cout<<cip_1<<" "<<cip_2<<endl;
        do{
            freopen("rule1.txt","r",stdin);
            check_1=ip_check(cip_1);
            check_2=ip_check(cip_2);
            cout<<check_1<<" "<<check_2<<endl;
            if(check_1&&check_2) break; //记得用judge换过来
            else if(check_1==-1){
                f=1;
                break;
            }
            else{
                cnt++;
            }
        }while(1);
        if(f) cout<<"-1";
        else cout<<cnt;
    }
    fclose(stdin);
    fclose(stdout);
    return 0;
}

int ip_chansform(void){//只读一条规则
    int a[6],step,i=0,j;
    char x,s[10],ex='0';
    while(cin>>x>>a[i]){
        ltoa(a[i],s,2);
        if(strlen(s)<8){
            for(j=0;j<8-strlen(s);j++){
                ip_rule+=ex;
            }
        }
        cout<<s<<endl;
        ip_rule+=s;
        if(i>=3) break;
        i++;
    }
    if(i<3) return -1; 
    cin>>x>>step;
    return step;
}

int ip_check(char a[]){
    ip_rule="";
    int t=ip_chansform(),i;
    cout<<t<<endl;
    cout<<ip_rule<<" ";
    if(t==-1) return -1;
    for(int i=0;i<t;i++){
        if(a[i]!=ip_rule[i]) break;
    }
    if(i==t) return 1;
    else return 0;
}

```
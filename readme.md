<H1>// SIMPLE PARITY CHECK</H1>
#include<bits/stdc++.h>
using namespace std;
bool checkEvenParity(string s){
    int par = 0;
    for(int i = 0;i<s.length();i++){
        if(s[i]=='1') par++;
    }
    return par%2==0;
}
int main(){
    string s;
    cin>>s;
    if(checkEvenParity(s)) s+='0';
    else s+='1';
    cout<<"Data to be sent to the receiver:"<<s<<endl;
    string r;
    cin>>r;
    if(checkEvenParity(r)) cout<<"accept"<<endl;
    else cout<<"reject"<<endl;
    return 0;
}



<H1>//Two Dimensional Parity Check</H1>
#include<bits/stdc++.h>
using namespace std;
bool checkEvenParity(string s){
    int par = 0;
    for(int i = 0;i<s.length();i++){
        if(s[i]=='1') par++;
    }
    return par%2==0;
}
int main(){
    string s;cin>>s;
    vector<string>sender;
    for(int i = 0;i<s.length();i+=8){
        sender.push_back(s.substr(i,8));
    }
    for(int i = 0;i<sender.size();i++){
        if(checkEvenParity(sender[i])) sender[i]+='0';
        else sender[i]+='1';
    }

    string l = "";
    for(int i = 0;i<sender[0].length();i++){
        string temp;
        for(int j = 0;j<sender.size();j++){
            temp+=sender[j][i];
        }
        if(checkEvenParity(temp)) l+='0';
        else l+='1';
    }
    cout<<l<<endl;
    sender.push_back(l);
    cout<<"Data to be sent:";
    for(auto it:sender) cout<<it;
    cout<<endl;
    string receiver;
    cin>>receiver;
    vector<string>rec;
    for(int i = 0;i<receiver.length();i+=9){
        rec.push_back(receiver.substr(i,9));
    }
    bool flag = true;
    for(int i = 0;i<rec.size();i++){
        if(!checkEvenParity(rec[i])) flag = false;
    }
    for(int i = 0;i<rec[0].length();i++){
        string temp = "";
        for(int j = 0;j<rec.size();j++){
            temp+=rec[j][i];
        }
        if(!checkEvenParity(temp)) flag=false;
    }
    if(flag) cout<<"accept"<<endl;
    else cout<<"reject"<<endl;
    return 0;
}



<H1>// CheckSum</H1>
#include<bits/stdc++.h>
using namespace std;
string checkSum(string s1,string s2){
    string ans = "";
    bool carry = false;
    for(int i = s1.length()-1;i>=0;i--){
        if(s1[i]=='0' && s2[i]=='0'){
            if(carry){
                ans='1'+ans;
                carry=false;
            }
            else{
                ans='0'+ans;
            }
        }
        else if(s1[i]=='1' && s2[i]=='1'){
            if(carry){
                ans='1'+ans;
            }
            else{
                ans='0'+ans;
                carry=true;
            }
        }
        else{
            if(carry){
                ans='0'+ans;
            }
            else{
                ans='1'+ans;
            }
        }
    }
    if(carry) ans=checkSum(ans,"00000001");
    return ans;
}
string complement(string s){
    string temp = "";
    for(int i = 0;i<s.length();i++){
        if(s[i]=='1') temp+='0';
        else temp+='1';
    }
    return temp;
}
int main(){
    string s;cin>>s;
    vector<string>sender;
    for(int i = 0;i<s.length();i+=8){
        sender.push_back(s.substr(i,i+8));
    }
    string add = sender[0];
    for(int i = 1;i<sender.size();i++){
        add=checkSum(add,sender[i]);
    }
    add=complement(add);
    string r;cin>>r;
    vector<string>rec;
    for(int i = 0;i<r.length();i+=8){
        rec.push_back(r.substr(i,8));
    }
    string radd = rec[0];
    for(int i = 1;i<rec.size();i++){
        radd=checkSum(radd,rec[i]);
    }
    radd=checkSum(radd,add);
    radd=complement(radd);
    bool flag = true;
    for(auto it:radd) if(it!='0') flag = false;
    if(flag) cout<<"accept"<<endl;
    else cout<<"reject"<<endl;
    return 0;
}



<H1>//Cyclic Redundancy Check</H1>
#include<bits/stdc++.h>
using namespace std;
int main(){
    string dividend;
    cin>>dividend;
    string divisor;
    cin>>divisor;
    int addZeros = divisor.length()-1;
    for(int i = 0;i<addZeros;i++) dividend+='0';
    string div = dividend.substr(0,divisor.length());
    bool start = true;
    for(int i = 0;i<dividend.length()-divisor.length();i++){
        if(start && dividend[i]=='0'){
            div=dividend.substr(i+1,divisor.length());
            continue;
        }
        start = false;
        string temp = "";
        if(div[0]!='0'){
            for(int j = 0;j<divisor.length();j++){
                if(div[j]==divisor[j]){
                    temp+='0';
                }
                else{
                    temp+='1';
                }
            }
            div=temp.substr(1,temp.length());
            div+=dividend[i+divisor.length()];
        }
        else{
            div=div.substr(1,div.length());
            div+=dividend[i+divisor.length()];
        }
    }
    div=div.substr(1,div.length());
    cout<<div<<endl;
    string r;cin>>r;
    r+=div;
    div=r.substr(0,divisor.length());
    start=true;
    for(int i = 0;i<r.length()-divisor.length();i++){
        if(start && r[i]=='0'){
            div=r.substr(i+1,divisor.length());
            continue;
        }
        start = false;
        string temp = "";
        if(div[0]!='0'){
            for(int j = 0;j<divisor.length();j++){
                if(div[j]==divisor[j]){
                    temp+='0';
                }
                else{
                    temp+='1';
                }
            }
            div=temp.substr(1,temp.length());
            div+=r[divisor.length()+i];
        }
        else{
            div=div.substr(1,div.length());
            div+=r[divisor.length()+i];
        }
    }
    cout<<"div::"<<div<<endl;
    div=div.substr(1,div.length());
    bool flag = true;
    for(auto it:div) if(it!='0') flag = false;
    if(flag) cout<<"accept"<<endl;
    else cout<<"reject"<<endl;
    return 0;
}
// 1101011011
// 10011


<H1>Hamming Code</H1>
#include<bits/stdc++.h>
using namespace std;
int main(){
    string input;
    cin>>input;
    int m = input.length();
    int p = 0;
    while(pow(2,p)<p+m+1) p++;
    int size = m+p;
    char arr[size+1];
    int k = m;
    int x = 0;
    for(int i = 1;i<=size;i++){
        if(pow(2,x)!=i){
            arr[i]=input[--k];
        }
        else{
            x++;
        }
    }
    x=0;
    for(int i = 1;i<=size;i++){
        if(pow(2,x)==i){
            x++;
            int par = 0;
            for(int j = i;j<=size;j+=2*i){
                for(int k = j;k<j+i && k<=size;k++){
                    if(k!=i){
                        if(arr[k]=='1') par++;
                    }
                }
            }
            if(par%2==0) arr[i]='0';
            else arr[i]='1';
        }
    }
    cout<<"Hamming Code:";
    for(int i = size;i>0;i--) cout<<arr[i];
    cout<<endl;
    string r;
    cin>>r;
    char arr1[size+1];
    k=r.length();
    for(int i = 1;i<=size;i++){
        arr1[i]=r[--k];
    }
    x=0;
    bool ans = true;
    string aa = "";
    for(int i = 1;i<=size;i++){
        if(pow(2,x)==i){
            x++;
            int par = 0;
            for(int j = i;j<=size;j+=2*i){
                for(int k = j;k<j+i && k<=size;k++){
                    if(arr1[k]=='1') par++;
                }
            }
            if(par%2!=0){
                ans=false;
                aa+='1';
            }
            else{
                aa+='0';
            }
        }
    }
    if(ans){
        cout<<"no error"<<endl;
    }
    else{
        cout<<"error at:"<<endl;
        int a = 0;
        for(int i = 0;i<aa.size();i++){
            if(aa[i]=='1'){
                a+=pow(2,i);
            }
        }
        cout<<a<<endl;
    }
    return 0;
}



<H1>//Stop and Wait ARQ</H1>
#include<bits/stdc++.h>
using namespace std;
int main(){
    cout<<"Enter the number of frames:"<<endl;
    int frames;
    cin>>frames;
    cout<<"Enter the nth frame lost:"<<endl;
    int lost;
    cin>>lost;
    cout<<"Enter the window size:"<<endl;
    int win;
    cin>>win;
    int f = 1;
    int count = 0;
    vector<int>ans;
    while(f<=frames){
        count++;
        ans.push_back(f);
        if(count==lost){
            count=1;
            ans.push_back(f);
        }
        f++;
    }
    for(auto it:ans) cout<<it<<" ";
    cout<<endl;
    return 0;
}



<H1>//Go Back N ARQ</H1>
#include<bits/stdc++.h>
using namespace std;
int main(){
    cout<<"Enter the number of frames to be sent:"<<endl;
    int frames;cin>>frames;
    cout<<"Enter the nth frame lost:"<<endl;
    int lost;cin>>lost;
    cout<<"Enter the window size:"<<endl;
    int win;cin>>win;
    int count = 0;
    int f = 1;
    vector<int>ans;
    while(f<=frames){
        ans.push_back(f);
        count++;
        if(count%lost==0){
            count=0;
            for(int j=f+1;j<f+win && j<=frames;j++){
                ans.push_back(j);
                count++;
            }
        }
        else{
            f++;
        }
    }
    for(auto it:ans) cout<<it<<" ";
    return 0;
}



<H1>//Selective Repeat ARQ</H1>
#include<bits/stdc++.h>
using namespace std;
int main(){
    cout<<"Enter the number of frames to be sent:"<<endl;
    int frames;cin>>frames;
    cout<<"Enter the nth frame that is lost:"<<endl;
    int lost;cin>>lost;
    cout<<"Enter the window size:"<<endl;
    int win;cin>>win;
    int f = 1;
    int count = 0;
    vector<int>ans;
    while(f<=frames){
        ans.push_back(f);
        count++;
        if(count==lost){
            count=0;
            int j = 0;
            for(j=f+1;j<f+win && j<=frames;j++){
                ans.push_back(j);
                count++;
            }
            ans.push_back(f);
            count++;
            f=j;
        }
        else{
            f++;
        }
    }
    for(auto it:ans) cout<<it<<" ";
}
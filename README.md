#include <stdio.h>
#include <stdlib.h>
#include <string.h>





typedef struct emploee{
	char id[15];
	char name[10];
	int  age;
	char work[40];
	char sex[6];
	char addres[40];
	char phone[15];
	char time[40];
}EMPL;


void  OutputMenu();                            //主菜单 
void  OutputSecondary();                       //子菜单 
void  InputInformation();                      //录入信息 
void  AddInformation();                        //添加信息 
void  DelInformation();                        //删除信息 
void  ChangeInformation();                     //更改信息 
void  InquireInformation();                    //查询信息 
void  ArrangementInformation();                //排列信息 
void  StatisticsInformation();                 //统计信息 
void  OutputInformation();                     //输出信息 
void  wsb();                                    //输出系统制作人信息
int   value(FILE *fp);                         //输出fp的信息量 






int main()//主函数
{
	system("color a0");
	system("mode con cols=50 lines=25");
	OutputMenu();
	return 0;
}






void  OutputMenu() //主菜单函数     
{                                                                        
    int number;
	printf("*************欢迎使用员工信息管理系统*************\n");
	printf("      1.录入员工信息          2.更新员工信息      \n");
	printf("      3.查询员工信息          4.排列员工信息      \n");
	printf("      5.统计员工信息          6.输出员工信息      \n");
	printf("      0.退出员工系统          8.员工系统简介      \n");
	printf("**************************************************\n");
	printf("请输入您选择功能项目前的数字，按回车继续！\n");
	scanf("%d",&number);
	switch(number){
		case 1 :
		    InputInformation();break;
		case 2 :
		    OutputSecondary();break;
		case 3 :
		    InquireInformation();break;
		case 4 :
		    ArrangementInformation();break;
		case 5 :
		    StatisticsInformation();break;
		case 6 :
		    OutputInformation();break;
        case 8 :
			wsb();break;
		case 0 :
		    break;
		default :
			printf("\a");OutputMenu();break;					
	}
	
}

void  OutputSecondary(){                        //子菜单                                                
	int number;
	printf("更新员工信息\n按1,添加员工信息\n按2,删除员工信息\n按3,修改员工信息\n按0,返回主菜单\n") ;
	scanf("%d",&number);
	switch(number){
		case 1 :
		    AddInformation();break;
		case 2 :
		    DelInformation();break;
		case 3 :
		    ChangeInformation();break;
		case 0 : 
		    OutputMenu();break;
		default :
			printf("\a");OutputSecondary();break;
		}
}

void  InputInformation(){                       //录入员工信息                                          
    EMPL empl;
    FILE *fp;
    system("cls");  //清屏函数 
	if((fp=fopen("emploee.txt","ab"))==NULL){
		printf("文件打开失败\a\n");
		exit(1);
	} 
    printf("请按顺序录入员工信息\n");
    while(1){
    	printf("请输入工号、姓名、年龄(18-64)、工作、性别、住址、电话、入职时间（年.月.日）\n(当结束输入后,请在首行按下Ctrl+z（~Z后回车返回主菜单）)\a\n");
    	fscanf(stdin,"%s%s%d%s%s%s%s%s",empl.id,empl.name,&empl.age,empl.work,empl.sex,empl.addres,empl.phone,empl.time);
    	if(feof(stdin))
    	    break;
        fwrite(&empl,sizeof(struct emploee),1,fp);
        printf("录入信息成功\n");    
	}
	fclose(fp);
	OutputMenu();
}

void  AddInformation(){                         //添加员工信息                                       
	EMPL empl;
	FILE *fp;
	system("cls");  //清屏函数 
	if((fp=fopen("emploee.txt","ab"))==NULL){
		printf("文件打开失败\a\n");
		exit(1);
	} 
    printf("请按顺序录入要添加的员工信息\n");
	while(1){
    	printf("请输入工号、姓名、年龄(18-64)、工作、性别、住址、电话、入职时间（年.月.日）\n(当结束输入后,请在首行按下Ctrl+z（~Z后回车返回子菜单）)\a\n");
    	fscanf(stdin,"%s%s%d%s%s%s%s%s",empl.id,empl.name,&empl.age,empl.work,empl.sex,empl.addres,empl.phone,empl.time);
    	if(feof(stdin))
    	    break;
        fwrite(&empl,sizeof(struct emploee),1,fp);
        printf("添加信息成功\n");
    }
    fclose(fp);
    OutputSecondary();
}
	
void  DelInformation(){                         //删除员工信息----- 工号                              
	EMPL empl,other;
	FILE *fp,*fp2;
	system("cls");  //清屏函数 
	if((fp=fopen("emploee.txt","rb"))==NULL){
		printf("文件打开失败\a\n");
		exit(1);
	} 
	if((fp2=fopen("emploeeChange.txt","ab"))==NULL){
		printf("文件打开失败\a\n");
		exit(1);
	} 
	printf("请输入需要删除的工号：");
	getchar();
	gets(other.id);
	while(1){
		
		fread(&empl,sizeof(struct emploee),1,fp);//把feof放在fread后才能阻止重复读取 
		if(feof(fp))
			    break;
		if(strcmp(other.id,empl.id)==0){
			if(feof(fp))
			    break;
			fread(&empl,sizeof(struct emploee),1,fp);
		
			
			printf("删除成功！\n");
			
			
		}
			
		fwrite(&empl,sizeof(struct emploee),1,fp2);
		    
	}
	fclose(fp);	
	fclose(fp2);
	remove("emploee.txt");
	rename("emploeeChange.txt","emploee.txt");
	OutputSecondary();
}

void  ChangeInformation(){                      //修改员工信息----- 工号                                
	EMPL empl,other;
	FILE *fp,*fp2;
	system("cls");  //清屏函数 
	if((fp=fopen("emploee.txt","rb"))==NULL){
		printf("emploee.txt文件打开失败\a\n");
		exit(1);
	} 
	if((fp2=fopen("emploeeChange.txt","ab"))==NULL){
		printf("emploeeChange.txt文件打开失败\a\n");
		exit(1);
	} 
	printf("请输入需要修改的工号：");
	getchar();
	gets(other.id);
	while(1){
		fread(&empl,sizeof(struct emploee),1,fp);
		if(feof(fp))
			    break; 
		if(strcmp(other.id,empl.id)==0){
			printf("请修改工号%s的信息：姓名、年龄、工作、性别(man or woman)、住址、电话、入职时间（年.月.日）\n",other.id);
			scanf("%s%d%s%s%s%s%s",empl.name,&empl.age,empl.work,empl.sex,empl.addres,empl.phone,empl.time);
           // fwrite(&empl,sizeof(struct emploee),1,fp);
            printf("修改信息成功\n");
		} 
		
		fwrite(&empl,sizeof(struct emploee),1,fp2);
	}
	fclose(fp);	
	fclose(fp2);
	remove("emploee.txt");
	rename("emploeeChange.txt","emploee.txt");
	OutputSecondary();
}

void  InquireInformation(){                     //查询员工信息----- 工号                        
	EMPL empl,other;
	FILE *fp;
	system("cls");  //清屏函数 
	if((fp=fopen("emploee.txt","rb"))==NULL){
		printf("文件打开失败\a\n");
		exit(1);
	} 
	printf("请输入需要查询的工号：");
	scanf("%s",other.id);
	while(1){
		if(feof(fp)){
			printf("该员工不存在！\n");
			break;
		}
		fread(&empl,sizeof(struct emploee),1,fp);
		if(strcmp(other.id,empl.id)==0){
		
		    printf("工号：%s\n姓名：%s\n年龄：%d\n工作：%s\n性别：%s\n住址：%s\n电话：%s\n入职：%s\n",empl.id,empl.name,empl.age,empl.work,empl.sex,empl.addres,empl.phone,empl.time);
	        break;
	    }
	}
	fclose(fp);	
	OutputMenu();
}

void  ArrangementInformation(){                 //排序员工信息                                                 
   	EMPL empl;
   	int a[47]={0,},i=18,j=0,sum;
	FILE *fp,*fp2;
	system("cls");  //清屏函数 
	if((fp=fopen("emploee.txt","rb"))==NULL){
		printf("emploee.txt文件打开失败\a\n");
		exit(1);
	} 
	if((fp2=fopen("temporary.txt","ab"))==NULL){
		printf("temporary.txt文件打开失败\a\n");
		exit(1);
	}
    while(1){
    	i=18;
    	j=0;
		fread(&empl,sizeof(struct emploee),1,fp);
		if(feof(fp))
		    break;
		for(;i<65;i++,j++){
			if(empl.age==i){
			    a[j]++;
			    break;
			}
		}
	}
	for(i=18,j=0,sum=0;i<65;i++,j++){
		rewind(fp);
		sum=0;
		for(;a[j]>sum;){
			fread(&empl,sizeof(struct emploee),1,fp);
			if(empl.age==i){
				sum++;
				fwrite(&empl,sizeof(struct emploee),1,fp2);
			}
		}
	}
	    
	fclose(fp);
	fclose(fp2); 
	remove("emploee.txt");
	rename("temporary.txt","emploee.txt");
	printf("排序结束\n");
	OutputMenu();
	
}

void  StatisticsInformation(){                  //统计员工信息                                                 
    ArrangementInformation(); //先进行员工信息排序 
    EMPL other,empl;
	FILE *fp,*fp2;
	int num;
	EMPL a[40];
	system("cls");  //清屏函数 
	if((fp=fopen("emploee.txt","rb"))==NULL){
		printf("文件打开失败\a\n");
		exit(1);
	} 
	if((fp2=fopen("temporaryAge.txt","wb"))==NULL){
		printf("文件打开失败\a\n");
		exit(1);
	}
	
	////////////////////////////////////////////////////年龄筛选          
	printf("请输入所要筛选出的年龄的个数：");
	scanf("%d",num);
	int i,j;
	for( i=0,j=0;i<num;i++,j++){
		printf("请输入要筛选的年龄：");
		scanf("%d",a[i].age);
	}
	printf("输入结束！\n");
    for(i=0;i<num;i++){	
        rewind(fp);
        while(!feof(fp)){
			fread(&other,sizeof(struct emploee),1,fp);
			if(a[i].age==other.age){
			   	printf("工号：%s\n姓名：%s\n年龄：%d\n工作：%s\n性别：%s\n住址：%s\n电话%s\n入职：%s\n",other.id,other.name,other.age,other.work,other.sex,other.addres,other.phone,other.time);
	 	   	    fwrite(&other,sizeof(struct emploee),1,fp2);
	 	   }
	   	}
   	}
	fclose(fp);
	fclose(fp2);
	
	
	if((fp2=fopen("temporarySex.txt","wb"))==NULL){///////////////////////////////////////////////////////////////////////////////////////////性别的筛选 
		printf("文件打开失败\a\n");
		exit(1);
	}
	if((fp=fopen("temporaryAge.txt","rb"))==NULL){
		printf("文件打开失败\a\n");
		exit(1);
	}
	
		printf("请输入要筛选的性别man or woman：");
		gets(empl.sex);
		printf("输入结束！\n");
		while(1){
			fread(&other,sizeof(struct emploee),1,fp);
			if(!feof(fp))
			    break;
			if(empl.sex==other.sex){
			    printf("工号：%s\n姓名：%s\n年龄：%d\n工作：%s\n性别：%s\n住址：%s\n电话%s\n入职：%s\n",other.id,other.name,other.age,other.work,other.sex,other.addres,other.phone,other.time);
	            fwrite(&empl,sizeof(struct emploee),1,fp2);
            }
	    }
    
    fclose(fp);
	fclose(fp2);
	
	if((fp2=fopen("temporarySex.txt","rb"))==NULL){///////////////////////////////////////////////////////////////////////////////////////////工作的筛选 
		printf("文件打开失败\a\n");
		exit(1);
	}
	if((fp=fopen("temporaryWork.txt","wb"))==NULL){
		printf("文件打开失败\a\n");
		exit(1);
	}
	
	printf("请输入需要筛选出的工作:"); 
	gets(empl.work);
	for(i=0;i<value(fp2);i++){
		fread(&other,sizeof(struct emploee),1,fp2);
		if(empl.work==other.work){
			printf("工号：%s\n姓名：%s\n年龄：%d\n工作：%s\n性别：%s\n住址：%s\n电话%s\n入职：%s\n",other.id,other.name,other.age,other.work,other.sex,other.addres,other.phone,other.time);
    		fwrite(&empl,sizeof(struct emploee),1,fp);
        }
	}
	fclose(fp);
	fclose(fp2);
	////////////////////////////////////输出筛选后的信息
	if((fp=fopen("temporaryWork.txt","rb"))==NULL){
		printf("文件打开失败\a\n");
		exit(1);
	}
	while(1){
		fread(&other,sizeof(struct emploee),1,fp2);
		if(!feof(fp))
			    break;
		printf("工号：%s\n姓名：%s\n年龄：%d\n工作：%s\n性别：%s\n住址：%s\n电话%s\n入职：%s\n",other.id,other.name,other.age,other.work,other.sex,other.addres,other.phone,other.time);
		fseek(fp,sizeof(struct emploee),1);
		
	}
	printf("以上信息保存在temporaryWork.txt文件中\n");
	fclose(fp);
	
	OutputMenu();
}

void  OutputInformation(){                      //输出员工信息                                                 ok 
	FILE *fp;
	EMPL empl;
	system("cls");  //清屏函数 
	if((fp=fopen("emploee.txt","rb"))==NULL){
		printf("文件打开失败\a\n");
		exit(1);
	} 
	fread(&empl,sizeof(struct emploee),1,fp);
	while(!feof(fp)){
		printf("工号：%s\n姓名：%s\n年龄：%d\n工作：%s\n性别：%s\n住址：%s\n电话：%s\n入职：%s\n",empl.id,empl.name,empl.age,empl.work,empl.sex,empl.addres,empl.phone,empl.time);
	    printf("该员工信息输出完毕！\n\n\n");
        fread(&empl,sizeof(struct emploee),1,fp);
	}
	printf("以上为全部员工信息！\n");
	fclose(fp);	
	OutputMenu();
}

int value(FILE *fp){                           //返回fp中有多少条信息                                          ok 
	int count=0;
	EMPL empl;
	while(1){
		fread(&empl,sizeof(struct emploee),1,fp);
		if(feof(fp))
		    break;
		count++;
	}
	rewind(fp);
	return count;
}







void  wsb()   
{
	system("cls");
	printf("制作人：王世博  王卓  王龙杰  梅老板  张宇  罗俊夫\n\n输入 0 返回主菜单\n\n");
	int x;
	scanf("%d",&x);
	if(x==0)
	{
		main();

	}
	
	
}

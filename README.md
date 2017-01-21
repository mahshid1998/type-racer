# type-racer
#include <stdio.h>
#include <stdlib.h>
#include <windows.h>
#include <string.h>
#include <time.h>
#include <ctype.h>
#include <conio.h>
typedef struct {
	char name[20];
	int scores[12];
}user_info;
typedef struct node {
	char vocab[20];
	struct node *next;
}node_t;
typedef struct information {
	char name[20];
	int arr[20];
	struct information *next;
}info_t;
clock_t duration;
float score;
int files=1;
info_t *start_info = NULL;
void save(user_info *user)
{
	int i , chert=0;
	FILE*fp;
	fp= fopen("scores.txt", "w");
	while(start_info !=NULL)
	{
		if(strcmp((*start_info).name, user->name)==0)
			start_info=start_info->next;
		fprintf(fp,"\n");
		fprintf(fp,"#");
		fprintf(fp, "%s ", (*start_info).name);
		for(i=1;i<=files;i++)
			fprintf(fp, "%d ",(*start_info).arr[i]);
		start_info=start_info->next;
	}
		fprintf(fp,"#");
		fprintf(fp, "%s ", (*user).name);
		for(i=1;i<=files;i++)
			fprintf(fp, "%d ",(*user).scores[i]);
}
int gotoxy(int x,int y)
{
    COORD coord = {x,y};
    SetConsoleCursorPosition(GetStdHandle(STD_OUTPUT_HANDLE),coord);
}
void print_check(user_info *user , node_t *head)
{
	clock_t start, end , pause=0 , resume=0 ;
	int chert=0 , current=0 , i=0 , color=9 , fals=0 , x=0 , y=0;
	char c , user_word[10] , word[20];
 	strcpy(word,head->vocab);
 	head = head->next;
	start = clock();
	while(chert<=strlen(word))
	{
		if(chert==strlen(word))
 			break;
		system("cls");
		HANDLE hstdout = GetStdHandle(STD_OUTPUT_HANDLE);
		gotoxy( x, y);
		for(i=0;i<current;i++)
		{
			SetConsoleTextAttribute(hstdout,10);
			printf("%c",word[i]);
		}
		if(i==current)
		{
			SetConsoleTextAttribute(hstdout,color);
			printf("%c",word[i]);
		}
		i++;
		for(;i<strlen(word);i++)
		{
			SetConsoleTextAttribute(hstdout,15);
			printf("%c",word[i]);
		}
		printf("\n");
		for(i=0;i<current;i++)
		{
			SetConsoleTextAttribute(hstdout,13);
 			printf("%c",user_word[i]);
		}
		SetConsoleTextAttribute(hstdout,13);
   		c = getche ();
 		if(c=='Q')
 		{
 			save(user);
			exit(0);
		}
		if(c=='P')
		{
			pause = clock();
			fals--;
			while(c!='R')
			{
				scanf("%s",&c);
			}
		}
		if(c=='R')
		{
			resume = clock();
			fals--;
		}
		if(c==word[current])
		{
			user_word[i]=c;
			word[i]=toupper(word[i]);
 			current++;
			color=9;
		}
		else if(c!=word[current])
		{
			fals++;
			color=12;
			chert--;
		}
	chert++;
	x++;
	y++;
 	}
 	end = clock();
 	if(pause!=0)
 		duration=(double)((pause - start) + (end - resume)) / CLOCKS_PER_SEC;
	else
 		duration=(double)(end - start) / CLOCKS_PER_SEC;
 	score+=(double)(strlen(word)*3-fals)/duration;
  }
void another_score(user_info *user)
{
	int i = 1  , m=0 , count_wordsfile = 0 , SCORE_AVR = 0 ;
	char str1[20]="level-" , str2[10], str3[]=".txt" , str4[10] , c ;
	FILE *fd;
	while(i<=files)
	{
		if(user->scores[i]!=-1)
		{
   		 	sprintf(str2,"%d",i);
   			strcat(str2,str3);
   			strcat(str1,str2);
    		fd=fopen(str1,"r");
			//!!!!!!!!!!!!!!!!!!!!number of file words!!!!!!!!!!!!!!!
 			while((c = fgetc(fd)) != EOF)
			{
    			if(c == ' ')
            		count_wordsfile++;
   	 		}
   	 		fclose(fd);
   	 		SCORE_AVR+=count_wordsfile*user->scores[i] ;
   	 		m += count_wordsfile;
		}
		str2[10]=str4[10];
		strcpy(str1 , "level-") ;
		i++ ;
	}
	SCORE_AVR = (double)SCORE_AVR /(double) m ;
	printf("SCORE_AVR = %d\n",SCORE_AVR);

}
void opening_file_level(int level , user_info *user)
{
	int count_wordsfile = 0, which_word , i , k=1 , chert1=1 , sum=0 , chert2=1 , wpm;
  	char c , word[10] , chert[20] ;//for finding the words numberers in file
	FILE *fp;
	node_t *head = NULL , *start_list= NULL;
	  		//!!!!!!!!!!!!!!!!!!!!loading message!!!!!!!!!!!!!!!
	HANDLE hstdout = GetStdHandle(STD_OUTPUT_HANDLE);
	SetConsoleTextAttribute(hstdout,13);
	for(i=0;i<44;i++)
		printf("%c",219);
	printf("\n");
	printf("%c",219);
	SetConsoleTextAttribute(hstdout,12);
	printf("!!!!!!!!!!!!!!!!!!!!loading!!!!!!!!!!!!!!!");
	SetConsoleTextAttribute(hstdout,13);
	printf("%c",219);
	printf("\n");
 //	SetConsoleTextAttribute(hstdout,13);
  	for(i=0;i<44;i++)
		printf("%c",219);
   		//!!!!!!!!!!!!!!!!!!!!opening file!!!!!!!!!!!!!!!
 	char str1[20]="level-" , str2[10], str3[]=".txt" , str4[10] ;
    sprintf(str2,"%d",level);
   	strcat(str2,str3);
   	strcat(str1,str2);
    fp=fopen(str1,"r");
		//!!!!!!!!!!!!!!!!!!!!number of file words!!!!!!!!!!!!!!!
 	while((c = fgetc(fp)) != EOF)
	{
    	if(c == ' ')
            count_wordsfile++;
    }
    rewind(fp);
    		//!!!!!!!!!!!!!!!!!!!!read words !!!!!!!!!!!!!!!
    for(i=1;i<=count_wordsfile;i++)
		{
 			fscanf(fp,"%s",&word);
		}
		head = (node_t *) malloc(sizeof(node_t));
		start_list = (node_t *) malloc(sizeof(node_t));
		start_list = head;
		head->next = (node_t *) malloc(sizeof(node_t));
		strcpy(head->vocab,word);
		head = head->next;
		head->next = NULL;
    for(i=1;i<=20;i++)
    	chert[i]=0;
    while(chert2<count_wordsfile)
    {
    	while(chert1==1)
    	{
    		srand(time(NULL));
    		which_word=(rand()%count_wordsfile);
    		for(i=1;i<=20;i++)
    	 	{
    	 		if(chert[i]!=which_word)
    	 			sum++;
		 	}
		 	if(sum==20)
		 	{
		 		chert[k]=which_word;
		 		k++;
		 		chert1=0;
		 	}
		 	sum=0;
		}
		chert1 = 1;
		rewind(fp);
		for(i = 1; i <= which_word;i++)
		{
 			fscanf(fp, "%s", &word);
		}
		head->next = (node_t *) malloc(sizeof(node_t));
		strcpy(head->vocab,word);
 		head = head->next;
		head->next = NULL;
     	chert2++;
    	rewind(fp);
	}
	while(start_list->next != NULL)
	{
		print_check(user,start_list);
		start_list = start_list->next;
	}
 	score=score/(double)(count_wordsfile);
 	user->scores[level]=score;
 	wpm =10* (double)(count_wordsfile)/(double)(duration);
 	system("cls");
  	SetConsoleTextAttribute(hstdout,15);
	printf("%s\n",user->name);
	printf("wpm=%d\n",wpm);
	printf("score=%d\n",user->scores[level]);
	another_score(user);
	fclose(fp);
}
void chosing_level(user_info *user,int new_or_resume , char name[20])
{

	HANDLE hstdout = GetStdHandle(STD_OUTPUT_HANDLE);
	int i, level, chert=1 , sum = 0  , x , y = 0 , cheshmak , k ;
	char c='*';
	FILE *fp;
	if(new_or_resume==1)
	{
		for(i=1;i<14;i++)
			user->scores[i]=-1;
	}
	else if(new_or_resume==2)
	{
		fp = fopen("scores.txt", "a+");
		rewind(fp);
 		while(chert==1)
		{
			while(c!='#')
				fscanf(fp, "%c",&c);
 			fscanf(fp, "%s",&user->name);
 			if(strcmp(name,user->name)==0)
			{
				chert=0;
				for(i=1;i<=files;i++)
					fscanf(fp,"%d",&user->scores[i]);
			}
  			c='*';
 		}
	}
	strcpy(user->name,name);
	chert=1;
	while(chert==1)
	{
		SetConsoleTextAttribute(hstdout,151);
		x=10 ;
		gotoxy( x, y);
		printf("chose a level 1 to %d\n", files);
		x = 5 ;
		y++ ;
		gotoxy( x, y);
		SetConsoleTextAttribute(hstdout,110);
		if(new_or_resume != 1)
		{
			printf("the levels that you played:");
				for(i=1;i<=files;i++)
				{
					if(((*user).scores[i])!=-1)
					{
						printf("%d,",i);
						sum++;
					}
				}
		printf("\n");
		}
		if(sum == files)
		{
			SetConsoleTextAttribute(hstdout,11);
			for(cheshmak = 0 ; cheshmak < 100 ; cheshmak++)
			{
 				SetConsoleTextAttribute(hstdout,cheshmak);
				printf("levels compeleted\n");
 				system("cls");
			}
 			SetConsoleTextAttribute(hstdout,13);
			for(k=0;k<19;k++)
				printf("%c",219);
			printf("\n");
			printf("%c",219);
			SetConsoleTextAttribute(hstdout,225);
			printf("levels compeleted");
			SetConsoleTextAttribute(hstdout,13);
			printf("%c ",219);
			printf("\n");
			for(k=0;k<19;k++)
			printf("%c",219);
			printf("\n\n");
			save(user);
			exit (0);
		}
		scanf("%d",&level);
		if(level>files || user->scores[level]!=-1)
			printf("!!!!!!wrong level!!!!!!\n");
		else
			chert=0;
		Sleep(1000);
		system("cls");
	}

	SetConsoleTextAttribute(hstdout,15);
	opening_file_level(level,user);

}
void get_user_info(	user_info *user)
{
	int i,new_or_resume , chert=1;
	char name[20];
 	//!!!!!!!!!!!!!!!!!!!!welcome messsage!!!!!!!!!!!!!!!
	HANDLE hstdout = GetStdHandle(STD_OUTPUT_HANDLE);
	SetConsoleTextAttribute(hstdout,13);
	for(i=0;i<28;i++)
		printf("%c",219);
	printf("\n");
	printf("%c",219);
	SetConsoleTextAttribute(hstdout,225);
	printf(" welcome to the typeracer ");
	SetConsoleTextAttribute(hstdout,13);
	printf("%c ",219);
	printf("\n");
	for(i=0;i<28;i++)
		printf("%c",219);
	printf("\n\n");
	//!!!!!!!!!!!!!!!!!!!!scanning useer infrmation!!!!!!!!!!!!!!!
 	for(i=0;i<29;i++)
		printf("%c",210);
	printf("\n");
	SetConsoleTextAttribute(hstdout,10);
	printf("plz enter a name as your user\n");
	SetConsoleTextAttribute(hstdout,13);
	for(i=0;i<29;i++)
		printf("%c",210);
	printf("\n");
	SetConsoleTextAttribute(hstdout,15);
	scanf("%s",&name);
	SetConsoleTextAttribute(hstdout,5);
	for(i=0;i<(48+ strlen(name));i++)
		printf("%c",220);
	printf("\n");
 	SetConsoleTextAttribute(hstdout,3);
	printf("Hi %s play a new game [1] or resume an old game[2]\n",name);
	SetConsoleTextAttribute(hstdout,5);
	for(i=0;i<(48+ strlen(name));i++)
		printf("%c",220);
	printf("\n");
	SetConsoleTextAttribute(hstdout,15);
	scanf("%d",&new_or_resume);
	system("cls");
	chosing_level(user, new_or_resume, name);
	//!!!!!!!!!!!!!!!!!!!!resume!!!!!!!!!!!!!!!
	while(1)
	{
		printf("another level???? yes 3 or no 0\n");
		scanf("%d",&new_or_resume);
		if(new_or_resume==3)
		{
			system("cls");
			chosing_level(user, new_or_resume , name);
		}
 		else if(new_or_resume==0)
 		{
 			save(user);
			exit(0);
		}

	}
 }
void print_top_10()
{
	int chert ;
	info_t *start_list ;
	start_list = (info_t *) malloc(sizeof(info_t));
	start_list = start_info ;
	for(chert=0 ; chert<100 ; chert++)
	{
 			printf("!!!!!!!!!!!!!!!!scores!!!!!!!!!!!!!!!!!!!");
 			system("cls");
 	}

	while(start_list != NULL)
	{
		if(start_list->arr[0]<500 && start_list->arr[0]>0)
		{
			printf("%s score = %d\n",start_list->name , start_list->arr[0]);
			for(chert=0; chert<20;chert++)
				printf("%c",220);

		}
		printf("\n");
		start_list = start_list->next ;
	}
	printf("click to start\n") ;
	getche();
 	system("cls") ;
}
void Top_10()
{
	int i, j, k , chert = 0 ;
    info_t *p, *q , *temp ;
    k = files ;
    for ( i = 0 ; i < files; i++, k-- )
    {
        p = start_info ;
        q = p -> next ;
        for ( j = 1 ; j < k ; j++ )
        {
             if ( p -> arr[0] < q -> arr[0] )
            {
                for(chert=0; chert<=files; chert++)
                    temp->arr[chert] = p -> arr[chert] ;
                strcpy(temp ->name , p ->name );
                for(chert=0; chert<=files; chert++)
               		p -> arr[chert] = q -> arr[chert] ;
                strcpy(p -> name  , q -> name) ;
                 for(chert=0; chert<files; chert++)
                    q -> arr[chert] = temp->arr[chert] ;
               strcpy(q ->name , temp->name) ;
             }
             	p = p -> next ;
            	q = q -> next ;	
        }

     }
     print_top_10();
}
void file_information()
{
	int i, chert=1;
	info_t *first = NULL;
	char c='*';
	FILE *fp;
	first = (info_t *) malloc(sizeof(info_t));
    start_info = (info_t *) malloc(sizeof(info_t));
	start_info = first;
 	fp= fopen("scores.txt", "r");
 	while(!feof(fp))
	{
		while(c!='#')
		{
			fscanf(fp, "%c",&c);
			if (feof(fp))
			{
				chert=0;
				break;

			}
		}
		if(chert == 1)
		{
 			fscanf(fp,"%s",&first->name);
   			for(i=1;i<=files;i++)
 				fscanf(fp,"%d",&first->arr[i]);
 			for(i=1;i<=files;i++)
 				first->arr[0]+=first->arr[i];
 			for(i=1;i<=files;i++)
 			{
 				if(first->arr[i]==-1)
 					first->arr[0]++;
			}
   			first->next = (info_t *) malloc(sizeof(info_t));
 			first = first->next;
			first->next = NULL;
			c='*';
 		}
	}
}
void number_of_files()
{
	char str1[20]="level-" ,  str2[10] , str3[]=".txt" , str4[10] ;
	int  chert = 1 ;
	FILE *fp ;
	while(chert == 1)
   {
		sprintf(str2, "%d", files) ;
   		strcat(str2, str3) ;
   		strcat(str1, str2) ;
   		fp = fopen(str1, "r") ;
   		if (fp == NULL)
		 	chert = 0 ;
		strcpy(str1, "level-");
		files++ ;
		strcpy(str2, str4) ;
		fclose(fp) ;
   }
   files =  files - 2;
}
int main()
{
   user_info user;
   number_of_files();
   file_information();
   Top_10() ;
   get_user_info(&user);
  return 0;
}

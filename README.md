# Expt 5 
fcfs
```
/* Program to Simulate First Come First Serve CPU Scheduling Algorithm */
#include<stdio.h>
#include<string.h>
void main()
{
int i,j,n,bt[10],compt[10],at[10], wt[10],tat[10];
float sumwt=0.0,sumtat=0.0,avgwt,avgtat;
printf("Enter number of processes: ");
scanf("%d",&n);
printf("Enter the burst time of %d process\n", n);
for(i=0;i<n;i++)
{
scanf("%d",&bt[i]);
}
printf("Enter the arrival time of %d process\n", n);
for(i=0;i<n;i++)
{
scanf("%d",&at[i]);
}
compt[0]=bt[0]-at[0];
for(i=1;i<n;i++)
compt[i]=bt[i]+compt[i-1];
for(i=0;i<n;i++)
{
tat[i]=compt[i]-at[i];
wt[i]=tat[i]-bt[i];
sumtat+=tat[i];
sumwt+=wt[i];
}
avgwt=sumwt/n;
avgtat=sumtat/n;
printf("----------------------------------\n");
printf("At\t PN\tBt\tCt\tTat\tWt\n");
printf("----------------------------------\n");
for(i=0;i<n;i++)
{
printf(" %d\t%d\t%2d\t%2d\t%2d\t%2d\n",at[i],i,bt[i],compt[i],tat[i],wt[i]);
}
printf("----------------------------------\n");
printf(" Avgwt = %.2f\tAvgtat = %.2f\n",avgwt,avgtat);
printf("-----------------------------------\n");
}
```
sjf
```
#include<string.h>
#include<stdio.h>
int main()
{
 int bt[20],at[10],n,i,j,temp,st[10],ft[10],wt[10],ta[10];
 int totwt=0,totta=0;
 double awt,ata;
 char pn[10][10],t[10];
 
 printf("Enter the number of process:");
 scanf("%d",&n);
 for(i=0; i<n; i++)
 {
 printf("Enter process name, arrival time& burst time:");
 scanf("%s%d%d",pn[i],&at[i],&bt[i]);
 }
 for(i=0; i<n; i++)
 for(j=0; j<n; j++)
 {
 if(bt[i]<bt[j])
 {
 temp=at[i];
 at[i]=at[j];
 at[j]=temp;
 temp=bt[i];
 bt[i]=bt[j];
 bt[j]=temp;
 strcpy(t,pn[i]);
 strcpy(pn[i],pn[j]);
 strcpy(pn[j],t);
 }
 }
 for(i=0; i<n; i++)
 {
 if(i==0)
 st[i]=at[i];
 else
 st[i]=ft[i-1];
 wt[i]=st[i]-at[i];
 ft[i]=st[i]+bt[i];
 ta[i]=ft[i]-at[i];
 totwt+=wt[i];
 totta+=ta[i];
 }
 awt=(double)totwt/n;
 ata=(double)totta/n;
 printf("\nProcessname\tarrivaltime\tbursttime\twaitingtime\tturnaroundtime");
 for(i=0; i<n; i++)
 {
 printf("\n%s\t%5d\t\t%5d\t\t%5d\t\t%5d",pn[i],at[i],bt[i],wt[i],ta[i]);
 }
 printf("\nAverage waiting time: %f",awt);
 printf("\nAverage turnaroundtime: %f",ata);
 return 0;
}
```

# expt 6 producer consumer problem
```
#include<stdio.h>
#include<stdlib.h>
 
int mutex=1,full=0,empty=3,x=0;
 
int main()
{
    int n;
    void producer();
    void consumer();
    int wait(int);
    int signal(int);
    printf("\n1.Producer\n2.Consumer\n3.Exit");
    while(1)
    {
        printf("\nEnter your choice:");
        scanf("%d",&n);
        switch(n)
        {
            case 1:    if((mutex==1)&&(empty!=0))
                        producer();
                    else
                        printf("Buffer is full!!");
                    break;
            case 2:    if((mutex==1)&&(full!=0))
                        consumer();
                    else
                        printf("Buffer is empty!!");
                    break;
            case 3:
                    exit(0);
                    break;
        }
    }
   
    return 0;
}
 
int wait(int s)
{
    return (--s);
}
 
int signal(int s)
{
    return(++s);
}
 
void producer()
{
    mutex=wait(mutex);
    full=signal(full);
    empty=wait(empty);
    x++;
    printf("\nProducer produces the item %d",x);
    mutex=signal(mutex);
}
 
void consumer()
{
    mutex=wait(mutex);
    full=wait(full);
    empty=signal(empty);
    printf("\nConsumer consumes item %d",x);
    x--;
    mutex=signal(mutex);
}
```

# expt 7 banker algo
```
// Banker's Algorithm  
#include <stdio.h>  
int main()  
{  
    // P0, P1, P2, P3, P4 are the Process names here  
  
    int n, m, i, j, k;  
    n = 5;                         // Number of processes  
    m = 3;                         // Number of resources  
    int alloc[5][3] = {{0, 1, 0},  // P0 // Allocation Matrix  
                       {2, 0, 0},  // P1  
                       {3, 0, 2},  // P2  
                       {2, 1, 1},  // P3  
                       {0, 0, 2}}; // P4  
  
    int max[5][3] = {{7, 5, 3},  // P0 // MAX Matrix  
                     {3, 2, 2},  // P1  
                     {9, 0, 2},  // P2  
                     {2, 2, 2},  // P3  
                     {4, 3, 3}}; // P4  
  
    int avail[3] = {3, 3, 2}; // Available Resources  
  
    int f[n], ans[n], ind = 0;  
    for (k = 0; k < n; k++)  
    {  
        f[k] = 0;  
    }  
    int need[n][m];  
    for (i = 0; i < n; i++)  
    {  
        for (j = 0; j < m; j++)  
            need[i][j] = max[i][j] - alloc[i][j];  
    }  
    int y = 0;  
    for (k = 0; k < 5; k++)  
    {  
        for (i = 0; i < n; i++)  
        {  
            if (f[i] == 0)  
            {  
                int flag = 0;  
                for (j = 0; j < m; j++)  
                {  
                    if (need[i][j] > avail[j])  
                    {  
                        flag = 1;  
                        break;  
                    }  
                }  
                if (flag == 0)  
                {  
                    ans[ind++] = i;  
                    for (y = 0; y < m; y++)  
                        avail[y] += alloc[i][y];  
                    f[i] = 1;  
                }  
            }  
        }  
    }  
    int flag = 1;  
    for (int i = 0; i < n; i++)  
    {  
        if (f[i] == 0)  
        {  
            flag = 0;  
            printf("The following system is not safe");  
            break;  
        }  
    }  
    if (flag == 1)  
    {  
        printf("Following is the SAFE Sequence\n");  
        for (i = 0; i < n - 1; i++)  
            printf(" P%d ->", ans[i]);  
        printf(" P%d", ans[n - 1]);  
    }  
    return (0);  
}  
```
# expt8
```
//BESTFIT MEMORY MANAGEMENT ALGORITHM IMPLEMENTATION
#include<stdio.h>
#include<conio.h>
#define max 25
void main()
{
int frag[max],b[max],f[max],i,j,nb,nf,temp,lowest=10000;
static int bf[max],ff[max];
printf("\nEnter the number of blocks:");
scanf("%d",&nb);
printf("Enter the number of Process:");
scanf("%d",&nf);
printf("\nEnter the size of the blocks:-\n");
for(i=1;i<=nb;i++)
{
printf("Block %d:\n",i);
scanf("%d",&b[i]);
}
printf("Enter the size of the Process :-\n");
for(i=1;i<=nf;i++)
{
printf("Process No %d:",i);
scanf("%d",&f[i]);
}
for(i=1;i<=nf;i++)
{
for(j=1;j<=nb;j++)
{
if(bf[j]!=1)
{
temp=b[j]-f[i];
if(temp>=0)
if(lowest>temp)
{
ff[i]=j;
lowest=temp;
}
}
}
frag[i]=lowest;
bf[ff[i]]=1;
lowest=10000;
}
printf("\nProcess No\tProcess Size \tBlock No\tBlock Size\tFragment");
for(i=1;i<=nf && ff[i]!=0;i++)
printf("\n%d\t\t%d\t\t%d\t\t%d\t\t%d",i,f[i],ff[i],b[ff[i]],frag[i]);
}



//FIRST FIT MEMORY MANAGEMENT ALGORITHM IMPLEMENTATION
#include<stdio.h>
void main()
{
int bsize[10], psize[10], bno, pno, flags[10], allocation[10], i, j;
for(i = 0; i < 10; i++)
{
flags[i] = 0;
allocation[i] = -1;
}
printf("Enter no. of blocks: ");
scanf("%d", &bno);
printf("\nEnter size of each block: ");
for(i = 0; i < bno; i++)
scanf("%d", &bsize[i]);
printf("\nEnter no. of processes: ");
scanf("%d", &pno);
printf("\nEnter size of each process: ");
for(i = 0; i < pno; i++)
scanf("%d", &psize[i]);
for(i = 0; i < pno; i++) //allocation as per first fit
for(j = 0; j < bno; j++)
if(flags[j] == 0 && bsize[j] >= psize[i])
{
allocation[j] = i;
flags[j] = 1;
break;
}
//display allocation details
printf("\nBlock no.\tsize\t\tprocess no.\t\tsize");
for(i = 0; i < bno; i++)
{
printf("\n%d\t\t%d\t\t", i+1, bsize[i]);
if(flags[i] == 1)
printf("%d\t\t\t%d",allocation[i]+1,psize[allocation[i]]);
else
printf("Not allocated");
}
}




\\lru
include<stdio.h>
#include<conio.h>
#define max 25
void main()
{
int frag[max],b[max],f[max],i,j,nb,nf,temp,highest=0;
static int bf[max],ff[max];
printf("\n\tMemory Management Scheme - Worst Fit");
printf("\nEnter the number of blocks:");
scanf("%d",&nb);
printf("Enter the number of files:");
scanf("%d",&nf);
printf("\nEnter the size of the blocks:-\n");
for(i=1;i<=nb;i++) {printf("Block %d:",i);scanf("%d",&b[i]);}
printf("Enter the size of the files :-\n");
for(i=1;i<=nf;i++) {printf("File %d:",i);scanf("%d",&f[i]);}
for(i=1;i<=nf;i++)
{
 for(j=1;j<=nb;j++)
 {
 if(bf[j]!=1) //if bf[j] is not allocated
 {
 temp=b[j]-f[i];
 if(temp>=0)
 if(highest<temp)
 {
 ff[i]=j;
 highest=temp;
 }
 }
 }
 frag[i]=highest;
 bf[ff[i]]=1;
 highest=0;
}
printf("\nFile_no:\tFile_size :\tBlock_no:\tBlock_size:\tFragement");
for(i=1;i<=nf;i++)
printf("\n%d\t\t%d\t\t%d\t\t%d\t\t%d",i,f[i],ff[i],b[ff[i]],frag[i]);
}
```
# expt 9
```
//FIFO PAGE REPLACEMENT
#include <stdio.h>
int main()
{
int referenceString[10], pageFaults = 0, m, n, s, pages, frames;
printf("\nEnter the number of Pages:\t");
scanf("%d", &pages);
printf("\nEnter reference string values:\n");
for( m = 0; m < pages; m++)
{
 printf("Value No. [%d]:\t", m + 1);
 scanf("%d", &referenceString[m]);
}
printf("\n What are the total number of frames:\t");
{
 scanf("%d", &frames);
}
int temp[frames];
for(m = 0; m < frames; m++)
{
 temp[m] = -1;
}
for(m = 0; m < pages; m++)
{
s = 0;
 for(n = 0; n < frames; n++)
 {
 if(referenceString[m] == temp[n])
 {
 s++;
 pageFaults--;
 }
 } 
 pageFaults++;
 if((pageFaults <= frames) && (s == 0))
 {
 temp[m] = referenceString[m];
 } 
 else if(s == 0)
 {
 temp[(pageFaults - 1) % frames] = referenceString[m];
 }
 printf("\n");
 for(n = 0; n < frames; n++)
 { 
 printf("%d\t", temp[n]);
 }
} 
printf("\nTotal Page Faults:\t%d\n", pageFaults);
return 0;
}


//##############################################################################################



include<stdio.h>
main()
{
int q[20],p[50],c=0,c1,d,f,i,j,k=0,n,r,t,b[20],c2[20];
printf("Enter no of pages:");
scanf("%d",&n);
printf("Enter the reference string:");
for(i=0;i<n;i++)
 scanf("%d",&p[i]);
printf("Enter no of frames:");
scanf("%d",&f);
q[k]=p[k];
printf("\n\t%d\n",q[k]);
c++;
k++;
for(i=1;i<n;i++)
 {
 c1=0;
 for(j=0;j<f;j++)
 {
 if(p[i]!=q[j])
 c1++;
 }
 if(c1==f)
 {
 c++;
 if(k<f)
 {
 q[k]=p[i];
 k++;
 for(j=0;j<k;j++)
 printf("\t%d",q[j]);
 printf("\n");
 }
 else
 {
 for(r=0;r<f;r++)
 {
 c2[r]=0;
 for(j=i-1;j<n;j--)
 {
 if(q[r]!=p[j])
 c2[r]++;
 else
 break;
 }
 }
 for(r=0;r<f;r++)
 b[r]=c2[r];
 for(r=0;r<f;r++)
 {
 for(j=r;j<f;j++)
 {
 if(b[r]<b[j])
 {
 t=b[r];
 b[r]=b[j];
 b[j]=t;
 }
 }
 }
 for(r=0;r<f;r++)
 {
 if(c2[r]==b[0])
 q[r]=p[i];
 printf("\t%d",q[r]);
 }
 printf("\n");
 }
 }
}
printf("\nThe no of page faults is %d",c);
}


//#################################################################################

```
# expt 10 
```
/*
 * FCFS Scheduling Program in C
 */
 
#include <stdio.h>
int main()
{
    int pid[15];
    int bt[15];
    int n;
    printf("Enter the number of processes: ");
    scanf("%d",&n);
 
    printf("Enter process id of all the processes: ");
    for(int i=0;i<n;i++)
    {
        scanf("%d",&pid[i]);
    }
 
    printf("Enter burst time of all the processes: ");
    for(int i=0;i<n;i++)
    {
        scanf("%d",&bt[i]);
    }
 
    int i, wt[n];
    wt[0]=0;
 
    //for calculating waiting time of each process
    for(i=1; i<n; i++)
    {
        wt[i]= bt[i-1]+ wt[i-1];
    }
 
    printf("Process ID     Burst Time     Waiting Time     TurnAround Time\n");
    float twt=0.0;
    float tat= 0.0;
    for(i=0; i<n; i++)
    {
        printf("%d\t\t", pid[i]);
        printf("%d\t\t", bt[i]);
        printf("%d\t\t", wt[i]);
 
        //calculating and printing turnaround time of each process
        printf("%d\t\t", bt[i]+wt[i]);
        printf("\n");
 
        //for calculating total waiting time
        twt += wt[i];
 
        //for calculating total turnaround time
        tat += (wt[i]+bt[i]);
    }
    float att,awt;
 
    //for calculating average waiting time
    awt = twt/n;
 
    //for calculating average turnaround time
    att = tat/n;
    printf("Avg. waiting time= %f\n",awt);
    printf("Avg. turnaround time= %f",att);
}

// c scan disk 


#include<stdio.h>
int main()
{
            int queue[20],n,head,i,j,k,seek=0,max,diff,temp,queue1[20],queue2[20],
                        temp1=0,temp2=0;
            float avg;
            printf("Enter the max range of disk\n");
            scanf("%d",&max);
            printf("Enter the initial head position\n");
            scanf("%d",&head);
            printf("Enter the size of queue request\n");
            scanf("%d",&n);
            printf("Enter the queue of disk positions to be read\n");
            for(i=1;i<=n;i++)
            {
                        scanf("%d",&temp);
                        if(temp>=head)
                        {
                                    queue1[temp1]=temp;
                                    temp1++;
                        }
                        else
                        {
                                    queue2[temp2]=temp;
                                    temp2++;
                        }
            }
            for(i=0;i<temp1-1;i++)
            {
                        for(j=i+1;j<temp1;j++)
                        {
                                    if(queue1[i]>queue1[j])
                                    {
                                                temp=queue1[i];
                                                queue1[i]=queue1[j];
                                                queue1[j]=temp;
                                    }
                        }
            }
            for(i=0;i<temp2-1;i++)
            {
                        for(j=i+1;j<temp2;j++)
                        {
                                    if(queue2[i]>queue2[j])
                                    {
                                                temp=queue2[i];
                                                queue2[i]=queue2[j];
                                                queue2[j]=temp;
                                    }
                        }
            }
            for(i=1,j=0;j<temp1;i++,j++)
            queue[i]=queue1[j];
            queue[i]=max;
            queue[i+1]=0;
            for(i=temp1+3,j=0;j<temp2;i++,j++)
            queue[i]=queue2[j];
            queue[0]=head;
            for(j=0;j<=n+1;j++)
            {
                        diff=abs(queue[j+1]-queue[j]);
                        seek+=diff;
                        printf("Disk head moves from %d to %d with seek                                                                           %d\n",queue[j],queue[j+1],diff);
            }
            printf("Total seek time is %d\n",seek);
            avg=seek/(float)n;
            printf("Average seek time is %f\n",avg);
            return 0;
}

// scan algorithm

#include <stdio.h>

#include <math.h>

int main()

{

    int queue[20], n, head, i, j, k, seek = 0, max, diff, temp, queue1[20], 
    queue2[20], temp1 = 0, temp2 = 0;

    float avg;

    printf("Enter the max range of disk\n");

    scanf("%d", &max);

    printf("Enter the initial head position\n");

    scanf("%d", &head);

    printf("Enter the size of queue request\n");

    scanf("%d", &n);

    printf("Enter the queue of disk positions to be read\n");

    for (i = 1; i <= n; i++)

    {

        scanf("%d", &temp);

        if (temp >= head)

        {

            queue1[temp1] = temp;

            temp1++;
        }

        else

        {

            queue2[temp2] = temp;

            temp2++;
        }
    }

    for (i = 0; i < temp1 - 1; i++)

    {

        for (j = i + 1; j < temp1; j++)

        {

            if (queue1[i] > queue1[j])

            {

                temp = queue1[i];

                queue1[i] = queue1[j];

                queue1[j] = temp;
            }
        }
    }

    for (i = 0; i < temp2 - 1; i++)

    {

        for (j = i + 1; j < temp2; j++)

        {

            if (queue2[i] < queue2[j])

            {

                temp = queue2[i];

                queue2[i] = queue2[j];

                queue2[j] = temp;
            }
        }
    }

    for (i = 1, j = 0; j < temp1; i++, j++)

        queue[i] = queue1[j];

    queue[i] = max;

    for (i = temp1 + 2, j = 0; j < temp2; i++, j++)

        queue[i] = queue2[j];

    queue[i] = 0;

    queue[0] = head;

    for (j = 0; j <= n + 1; j++)

    {

        diff = abs(queue[j + 1] - queue[j]);

        seek += diff;

        printf("Disk head moves from %d to %d with seek %d\n", queue[j], 
        queue[j + 1], diff);
    }

    printf("Total seek time is %d\n", seek);

    avg = seek / (float)n;

    printf("Average seek time is %f\n", avg);

    return 0;
}
```

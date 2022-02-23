#include <stdio.h>

void inputmat2(int l,int b,int ar[l][b])
{
  for(int i=0;i<l;i++)
  {
    printf("\n\t For P%d : ",i);
    for(int j=0;j<b;j++)
    {
      scanf("%d",&ar[i][j]);
    }
  }
}

void inputmat1(int l,int ar[l])
{
  for(int i=0;i<l;i++)
  {
    scanf("%d",&ar[i]);
  }
}

void findneed(int n1,int m1,int max1[n1][m1],int alloc1[n1][m1],int need1[n1][m1])
{
  for (int i = 0; i < n1; i++)
    {
        for (int  j = 0; j < m1; j++)
        {
            need1[i][j] = max1[i][j] - alloc1[i][j];
        }
    }
}

void safesequencefinder(int n1,int m1,int alloc1[n1][m1],int need1[n1][m1],int avail1[m1],int ans1[n1],int f1[n1])
{
    int y = 0,k,i,j,ind=0;
    for (k = 0; k < n1; k++)
    {
        for (i = 0; i < n1; i++)
        {
            if (f1[i] == 0)
            {
                int flag = 0;
                for (j = 0; j < m1; j++)
                {
                    if (need1[i][j] > avail1[j])
                    {
                        flag = 1;
                        break;
                    }
                }

                if (flag == 0)
                {
                    ans1[ind++] = i;
                    for (y = 0; y < m1; y++)
                        avail1[y] += alloc1[i][y];
                    f1[i] = 1;
                }
            }
        }
    }
}

int issafesequence(int n1,int f1[n1])
{
  for(int i=0;i<n1; i++)
    if(f1[i]==0)
      return 0;
    else
        return 1;
  return 1;
}
void safesequenceprinter(int n1,int ans1[n1])
{
  printf("\n\nFollowing is the SAFE Sequence\n");
  for (int i = 0; i < n1 - 1; i++)
  {
    printf(" P%d ->", ans1[i]);
  }
  printf(" P%d\n", ans1[n1 - 1]);
}

int main()
{
  int n,m,i,j,k;

  printf("\nEnter Number of processes : ");
  scanf("%d",&n );
  printf("\nEnter Number of resources : ");
  scanf("%d",&m );

  int alloc[n][m],max[n][m],avail[m];


  printf("\nEnter allocation matrix : \n");
  inputmat2(n,m,alloc);
  printf("\nEnter max matrix : \n");
  inputmat2(n,m,max);
  printf("\nEnter available matrix : ");
  inputmat1(m,avail);

  int need[n][m];
  findneed(n,m,max,alloc,need);

  int f[n], ans[n], ind = 0;
  for (k = 0; k < n; k++)
  {
    f[k] = 0;
  }

  safesequencefinder(n,m,alloc,need,avail,ans,f);
  if(issafesequence(n,f))
    safesequenceprinter(n,ans);
  else
    printf("\nSystem is not in safe state\n");
  return 0;
}

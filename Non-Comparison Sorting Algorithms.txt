//Non-Comparison Sorting Algorithms

#include<iostream>
#include<fstream>
#include<time.h>
#include<stdlib.h>
#include<iomanip>
#include<cmath>

using namespace std;

long int count=0;

//Counting Sort
void CountingSort(int *a, int n, int k)
{
	int *c;
	int *b;
	
	c = new int[k+1];
	b = new int[n+1];
	
	for(int i=0;i<=k;i++)
	{
		count++;
		c[i] = 0;
	}
	
	for(int i=1;i<=n;i++)
	{
		count++;
		c[a[i]] += 1; 
	}
	for(int i=1;i<=k;i++)
	{
		count++;
		c[i] += c[i-1];
	}
	
	for(int i=n;i>=1;i--)
	{
		count++;
		b[c[a[i]]] = a[i];
		c[a[i]] -= 1;
	}
	
	//Copying the sorted elements from b[] to a[]
	for(int i=1;i<=n;i++)
		a[i] = b[i];
	
		
	delete(c);
	delete(b);
}


//Radix Sort
//Extracting 'i'th digit from a number 'x'
int GetDigit(int x,int i)
{
	int d;
	x = x / pow(10,i-1);
	d = x % 10;
	return d;
}

//Stable sort
void CountingSort(int *a, int n, int k, int d)
{
	//n - size of a
	//k - counting sort property - maximum element value - here, 0 to 9
	//d - digit: sorting to be done on which digit(d)
	//d=1 - 1's place; 	d=2 - 10's place;	d=3 - 100's place; etc.,

	int *b;
	int *c;
	
	int s;
	
	b = new int[n+1];
	c = new int[k+1];
	
	for(int i=0;i<=k;i++)
	{
		count++;
		c[i] = 0;
	}
	
	for(int i=1;i<=n;i++)
	{
		count++;
		s = GetDigit(a[i],d);
		c[s] += 1; 
	}
	for(int i=1;i<=k;i++)
	{
		count++;
		c[i] += c[i-1];
	}
	
	for(int i=n;i>=1;i--)
	{
		count++;
		s = GetDigit(a[i],d);
		b[c[s]] = a[i];
		c[s] -= 1;
	}
	
	//Copying the sorted elements from b[] to a[]
	for(int i=1;i<=n;i++)
		a[i] = b[i];

	delete(c);
	delete(b);
}

void RadixSort(int *a, int n, int d)
{
	//i=1 - 1's place; 	i=2 - 10's place;	i=3 - 100's place;
	for(int i=1;i<=d;i++)
	{
		CountingSort(a,n,9,i);
	}
}

//For linked list ADT operations
struct Node
{
	double data;
	Node *next;	
};

class SList
{
	private:
		Node *a;
		int size;
	
	public:
		SList()
		{
			a = NULL;
			size = 0;
		}
		bool insertFront(double);		
		bool ElementAt(int,double&);
		int GetSize()	{	return size;		}
		void Sort(); 
};

//Funtion to obtain the element from the input position p
//Input: p - postion: 1,2,3,etc.,
//Output: x - Element in position p and True or False - Operation Success or Not
bool SList::ElementAt(int p, double &x)
{	
	Node *t = a;
	int c=1;
	while(t!=NULL)
	{
		if(c==p)
		{
			x = t->data;
			return true;
		}
		
		t = t->next;
		c++;
	}
	
	return false;
}

//Function to inserting new element x into the front of list
//Input x; Output: True or False - Operation Success or Not
bool SList::insertFront(double x)
{
	//New Node Creation
	Node *n;
 	n = new Node;
	
	if(n==NULL)
		return false;
	
	//Initialization
	n->data = x;
	n->next = NULL;
		
	//Link the New Node in the proper position	
	n->next = a;
	a = n;
	
	//Increase the size of the list
	size++;
	
	return true;

}

//Function to sorting elements in the list
void SList::Sort()
{
	Node *i,*j;
	double t;
	//Selection Sort Used here
	//For Insertion Sort, we need to traverse in reverse. 
	//For that, we need to use DLL instead of SLL.
	//Students may use DLL and Insertion Sort 

	for(i=a;i!=NULL;i=i->next)
	{
		for(j=i->next;j!=NULL;j=j->next)
		{
			if(j->data<i->data)
			{
				t = i->data;
				i->data = j->data;
				j->data = t;
			}
		}
	}
	
}

void BucketSort(double a[],int n)
{
	//Creating array of 'n' buckets
	SList *Buckets = new SList[n];
	
	//Inserting elements from a[] into different buckets
	int bNo;
	for(int i=1;i<=n;i++)
	{
		bNo = a[i]*n;
		Buckets[bNo].insertFront(a[i]);
		count++;
	}
	
	//Sorting each buckets
	for(int i=0;i<n;i++)
	{
		count++;
		Buckets[i].Sort();
	}
	
	//Concatenating buckets
	double t;
	for(int i=0,j=0;i<n;i++)
	{
		//cout<<"\nBucket Number: "<<i;
		//Buckets[i].display();
		for(int p=1;p<=Buckets[i].GetSize();p++)
		{
			count++;
			if(Buckets[i].ElementAt(p,t))
			{
				a[j++] = t;
			}
		}
	}
}

int main()
{
	cout<<showpoint<<setprecision(12);
	
	ofstream outf;
	ifstream inf;

	int n,k;
	int *a;
	
	cout<<"\nEnter n:";
	cin>>n;
	
	//Random numbers are going to be generated in the range 0 to n
	
	time_t stt;
	time(&stt);
	srand((long int)stt);
	
	//Loading numbers to input file
	outf.open("in.txt");
	for(int i=0;i<n;i++)
		outf<<"\t"<<rand()%(n+1);
	outf.close();
	
	//k is maximum element
	k=n;
	
	a = new int [n+1];
	
	//Counting Sort
	//Reading input in array from input file
		
	inf.open("in.txt");		
	for(int i=1;i<=n;i++)
	{
		inf>>a[i];
	}
	inf.close();
	
	count = 0;
	CountingSort(a,n,k); //k is maximum element in a[]
	
	//Writing sorted numbers to output file
	outf.open("CountingOut.txt");
	for(int i=1;i<=n;i++)
	{
		outf<<"\t"<<a[i];
	}
	outf.close();
	
	cout<<"\n\nCounting Sort - No. of Operations: "<<count;
	
	
	
	//Radix Sort
	//Reading input in array from input file
		
	inf.open("in.txt");		
	for(int i=1;i<=n;i++)
	{
		inf>>a[i];
	}
	inf.close();
	
	//d - No. of digits in maximum number n
	//Counting the maximum number of digits. Max value is k
	int tmp=k,d=0;
	while(tmp!=0)
	{
		d++;
		tmp/=10;
	}
	
	count = 0;
	RadixSort(a,n,d);
	
	//Writing sorted numbers to output file
	outf.open("RadixOut.txt");
	for(int i=1;i<=n;i++)
	{
		outf<<"\t"<<a[i];
	}
	outf.close();
	
	cout<<"\n\nRadix Sort - No. of Operations: "<<count;
	
	
	//Bucket Sort
	//Reading input in array from input file
	double *b = new double[n+1];	
	inf.open("in.txt");		
	for(int i=1;i<=n;i++)
	{
		inf>>a[i];
		//To make input integer number to a decimal within the range 0 to 1
		b[i] = a[i]/(double)n; 
	}
	inf.close();
	
	count = 0;
	BucketSort(b,n); 
	
	//Writing sorted numbers to output file
	outf.open("BucketOut.txt");
	for(int i=1;i<=n;i++)
	{
		outf<<"\t"<<b[i];
	}
	outf.close();
	
	cout<<"\n\nBucket Sort - No. of Operations: "<<count;

	cout<<"\n\n";
	delete(a);
	delete(b);

}

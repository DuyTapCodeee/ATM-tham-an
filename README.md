#include<stdio.h>
#include<string.h>
#include<stdlib.h>

typedef struct {
	char TenTien[50];
	int MG,PA;
}Tien;

void Swap(Tien *x, Tien *y){
	Tien temp = *x;
	*x=*y;
	*y=temp;
}

Tien* readFile(int *n){
	FILE *f=fopen("ATM.txt", "r");
	Tien* dslt=(Tien*)malloc(sizeof(Tien));
	int i=0;
	while(!feof(f)){
		fscanf(f, "%d",&dslt[i].MG);
		fgets(dslt[i].TenTien,50,f);
		dslt[i].TenTien[strlen(dslt[i].TenTien) - 1]='\0';
		dslt[i].PA=0;
		i++;
		dslt=(Tien*)realloc(dslt,sizeof(Tien)*(i+1)); 
	}
	*n=i;
	fclose(f);
	return dslt;
}

void bubblesort(Tien * dslt, int n){
	for(int i=0; i<=n-2; i++){
		for(int j=n-1; j>=i+1; j--){
			if(dslt[j].MG > dslt[j-1].MG)
			Swap(&dslt[j],&dslt[j-1]);
		}
	}
}

void inDS(Tien*dslt, int n, int tienrut){
	int tongtientra=0;
	printf("BAI TOAN ATM BANG GIAI THUAT THAM AN\n");
	printf("|---|------------------------------|---------|---------|----------|\n");
	printf("|%3s|%-30s|%-9s|%-9s|%-10s|\n","STT","Loai Tien","Menh Gia","So To","Thanh Tien");
	printf("|---|------------------------------|---------|---------|----------|\n");
	for(int i=0; i<n; i++){
		printf("|%3d|%-30s|%-9d|%-9d|%-10d|\n",i+1,dslt[i].TenTien,dslt[i].MG,dslt[i].PA,dslt[i].MG*dslt[i].PA);
		tongtientra+=dslt[i].MG*dslt[i].PA;
	}
	printf("|---|------------------------------|---------|---------|----------|\n");
	printf("So tien can rut la: %d\n",tienrut);
	printf("So tien da tra la: %d",tongtientra);
}

void thaman(Tien*dslt, int n, int tienrut){
	int i=0; 
	while(i<n){
		dslt[i].PA=tienrut/dslt[i].MG;
		tienrut-=dslt[i].MG*dslt[i].PA;
		i++;
	}
}

int main(){
	int n,k;
	Tien*dslt;
	dslt=readFile(&n);
	bubblesort(dslt,n);
	printf("Nhap so tien can rut: ");
	scanf("%d",&k);
	thaman(dslt,n,k);
	inDS(dslt,n,k);
	free(dslt);
	return 0;
}

#include <bits/stdc++.h>
#include <cmath>
using namespace std;

struct ip{

    int uno;
    int dos;
    int tres;
    int cuatro;

    int prefijo;
};

int menu()
{
    int x;
    cout<<"\tMenu\n\n";
    cout<<"1.- Subneteo\n";
    cout<<"2.- Salir\n\t>";
    cin>>x;
    return x;
}

void imprime(ip dir) //Direccion IP
{
    cout<<dir.uno<<"."<<dir.dos<<"."<<dir.tres<<"."<<dir.cuatro<<"/"<<dir.prefijo;
}

//A la direccion IP enviada se le deben sumar los host
ip sumar(ip dir, int x)
{
    int modulo,cociente;

    dir.cuatro+=x; 

    cociente=dir.cuatro/256;
    modulo=dir.cuatro%256;
    dir.cuatro=modulo; 

    dir.tres+=cociente; 
    cociente=dir.tres/256;
    modulo=dir.tres%256;
    dir.tres=modulo; 

    dir.dos+=cociente; 
    cociente=dir.dos/256;
    modulo=dir.dos%256;
    dir.dos=modulo;

    dir.uno+=cociente; 

    return dir;
}

void imprimeMascara(int x){

    for(int j=1;j<=4;j++)
    {
        if(x>=8)
        {
            x-=8;
            printf("255");
        }
        
        else
        {
            
            if(x==0)
            {
                cout<<"0";
            }else{
                int i=1,num=1;
                for(i=1;i<x;i++){
                    num=num<<1; 
                    num++;
                }
                for(i=x;i<8;i++){
                    num=num<<1; 
                }
                x=0;
                printf("%d",num);
            }
        }
        if(j<4)
            cout<<".";
    }
}

void subneteo(){
    ip direccion;
    int N; /// numero de ip para host
    int Nredes; ///Numero de redes en el subneting
    int sum=0;

    int hosts[102]; 
    int ips[102];   
    int bits[102];  
    int ipsTotales[102]; 
    ip direcciones[1002];

    cout<<"\n\n\tSubneteo de Redes\n\n";

   
    cout<<"Introduce la IP que utilizaras:\n";
    cout<<"\tEscribe el valor del octeto #1 en decimal: ";
    cin>>direccion.uno;
    cout<<"\tEscribe el valor del octeto #2 en decimal: ";
    cin>>direccion.dos;
    cout<<"\tEscribe el valor del octeto #3 en decimal: ";
    cin>>direccion.tres;
    cout<<"\tEscribe el valor del octeto #4 en decimal: ";
    cin>>direccion.cuatro;
    cout<<"\n\tEscribe el tamaño del prefijo: ";
    cin>>direccion.prefijo;
    cout<<"\n\t\t La ip que utilizaras es: ";
    imprime(direccion);
    cout<<"\n\n";
    

    N=pow(2,32-direccion.prefijo); ///Cálculo de los host maximos posibles

    ///Solicitar los host requeridos y la cantidad de subredes
    cout<<"Escribe el numero de subredes que necesitas: ";
    cin>>Nredes;
    for(int i=1;i<=Nredes;i++){
        cout<<"Escribe el numero de host de la red "<<i<<" : ";
        cin>>hosts[i];
        sum+=hosts[i];
    }
    


    if(sum<=N){
        //Tabla de Subteneo

        ip aux=direccion;

        direcciones[1]=direccion; ///La primera direccion es la misma que me dan, solo cambia prefijo
        ips[1]=hosts[1]+2;
        bits[1]=ceil(log2(ips[1])/log2(2));
        ipsTotales[1]=pow(2,bits[1]);
        direcciones[1].prefijo=32-bits[1];

        for(int i=2;i<=Nredes;i++){
            ips[i]=hosts[i]+2;
            bits[i]=ceil(log2(ips[i])/log2(2));
            ipsTotales[i]=pow(2,bits[i]);

            aux.prefijo=32-bits[i]; //Calculando el prefijo de la nueva subred
            aux=sumar(aux,ipsTotales[i-1]);

            direcciones[i]=aux;
        }
        

        printf("\n\n");
        //Tabla de subredes completa
        for(int i=1;i<=Nredes;i++)
        {
            printf("Subred %d: ",i);
            imprime(direcciones[i]);
            printf("\n\n");
            printf(" 1ra IP utillizable: ");
            imprime(sumar(direcciones[i],1));
            printf("\n\n");
            printf(" Ultima IP: ");
            imprime(sumar(direcciones[i],pow(2,bits[i])-2));
            printf("\n\n");
            printf(" Broadcast: ");
            imprime(sumar(direcciones[i],pow(2,bits[i])-1));
            printf("\n\n");
            printf(" Mascara de subred: ");
            imprimeMascara(direcciones[i].prefijo);
            printf("\n\n");
        }
        
    }else{
        cout<<"Error de subneteo \n\n";
    }
}

int main(){
    int op;
    while((op=menu())!=2){
        if(op==1){
            subneteo();
        }else{
            cout<<"Opcion invalida\n\n";
        }
    }
    cout<<"Presiona cualquier tecla para salir";
    getchar();
    getchar();
    return 0;
}

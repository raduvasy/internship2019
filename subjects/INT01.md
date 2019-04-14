**Given a user situated in location A, and 4 ATMS such that:**
1. The walking time between starting point and ATMs as well as between each ATMs is presented below:
    
    | From                 | To           | Duration (minutes) |
    |----------------------|--------------|--------------------|
    | User starting point  | ATM 1        | 5                  |
    | User starting point  | ATM 2        | 60                 |
    | User starting point  | ATM 3        | 30                 |
    | User starting point  | ATM 4        | 45                 |
    | ATM 1                | ATM 2        | 40                 |
    | ATM 1                | ATM 4        | 45                 |
    | ATM 2                | ATM 3        | 15                 |
    | ATM 3                | ATM 1        | 40                 |
    | ATM 3                | ATM 4        | 15                 |
    | ATM 4                | ATM 2        | 30                 |
    
2. Each ATM has the same amount of money equal with 5000 lei, and the following schedule:

    | ATM Name | Opening time | Closing time |
    |----------|--------------|--------------|
    | ATM 1    | 12:00        | 18:00        |
    | ATM 2    | 10:00        | 17:00        |
    | ATM 3    | 22:00        | 12:00        |
    | ATM 4    | 17:00        | 01:00        |

3. User has 3 credit cards with the following properties:

    | Credit card | Fee  | Withdraw limit (lei) / day  | Expiration date| Available Amount (lei)|
    |-------------|------|-----------------------------|----------------|-----------------------|
    | SILVER      | 0.2% | 4500                        | 23.05.2020     | 20000                 |
    | GOLD        | 0.1% | 3000                        | 15.08.2018     | 25000                 |
    | PLATINUM    | 0%   | 4000                        | 20.03.2019     | 3000                  |

4. Current date and time is: *19th March, 2019 - 11:30*

**Please write an application using either Java or C# programming language in order to:**
1. <h5>Subject code = INT01 </h5>
 Determine what ATMs and in which order the user needs to access for withdrawing 7500 lei until 19th March, 2019 - 14:00, with smallest fee available,
using only valid credit cards (that are not expired at the current date),
without passing through the same location twice.
Processing time for each ATM is negligible.

The application should implement the following method:

    public List<Atm> getAtmsRoute() {
        // TODO: add your code here
    }


**Note:**

1. *All dates are in the same timezone.*
2. *Only specified constraints are to be taken in consideration.*




//Petretchi Vasile Radu

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace ATMApp
{
    class Program
    {
        static void Main(string[] args)
        {
            //table with walking time between starting point and 
            //ATMs as well as between each ATMs
            int[,] TabTime = new int[8,5];
            TabTime[0, 0] = 30;
            TabTime[0, 1] = 40;
            TabTime[0, 2] = 15;
            TabTime[0, 3] = 15;
            TabTime[1, 0] = 30;
            TabTime[1, 1] = 45;
            TabTime[1, 2] = 30;
            TabTime[1, 3] = 15;
            TabTime[2, 0] = 60;
            TabTime[2, 1] = 15;
            TabTime[2, 2] = 40;
            TabTime[2, 3] = 45;
            TabTime[3, 0] = 60;
            TabTime[3, 1] = 15;
            TabTime[3, 2] = 15;
            TabTime[4, 0] = 30;
            TabTime[4, 1] = 40;
            TabTime[4, 2] = 40;
            TabTime[5, 0] = 30;
            TabTime[5, 1] = 40;
            TabTime[5, 2] = 45;
            TabTime[5, 3] = 30;
            TabTime[6, 0] = 30;
            TabTime[6, 1] = 15;
            TabTime[6, 2] = 30;
            TabTime[7, 0] = 40;
            TabTime[7, 1] = 30;
            TabTime[7, 2] = 15;

            //table with ATM's name and all possible routes in order
            string [,] TabName = new string[8,5];
            TabName[0, 0] = "ATM 1";
            TabName[0, 1] = "ATM 2";
            TabName[0, 2] = "ATM 3";
            TabName[0, 3] = "ATM 4";
            TabName[1, 0] = "ATM 1";
            TabName[1, 1] = "ATM 4";
            TabName[1, 2] = "ATM 2";
            TabName[1, 3] = "ATM 3";
            TabName[2, 0] = "ATM 2";
            TabName[2, 1] = "ATM 3";
            TabName[2, 2] = "ATM 1";
            TabName[2, 3] = "ATM 4";
            TabName[3, 0] = "ATM 2";
            TabName[3, 1] = "ATM 3";
            TabName[3, 2] = "ATM 4";
            TabName[4, 0] = "ATM 3";
            TabName[4, 1] = "ATM 1";
            TabName[4, 2] = "ATM 2";
            TabName[5, 0] = "ATM 3";
            TabName[5, 1] = "ATM 1";
            TabName[5, 2] = "ATM 4";
            TabName[5, 3] = "ATM 2";
            TabName[6, 0] = "ATM 3";
            TabName[6, 1] = "ATM 4";
            TabName[6, 2] = "ATM 2";
            TabName[7, 0] = "ATM 4";
            TabName[7, 1] = "ATM 2";
            TabName[7, 2] = "ATM 3";
            
            //a table that check if ATM's form routes are open 
           bool[,] isOpen = new bool[8,5];
            isOpen[0, 0] = true;
            isOpen[0, 1] = true;
            isOpen[0, 2] = false;
            isOpen[0, 3] = false;
            isOpen[1, 0] = false;
            isOpen[1, 1] = false;
            isOpen[1, 2] = false;
            isOpen[1, 3] = false;
            isOpen[2, 0] = false;
            isOpen[2, 1] = false;
            isOpen[2, 2] = false;
            isOpen[2, 3] = false;
            isOpen[4, 0] = false;
            isOpen[4, 1] = false;
            isOpen[4, 2] = false;
            isOpen[5, 0] = false;
            isOpen[5, 1] = false;
            isOpen[5, 2] = false;
            isOpen[6, 0] = false;
            isOpen[6, 1] = false;
            isOpen[6, 2] = false;
            isOpen[7, 0] = false;
            isOpen[7, 1] = false;
            isOpen[7, 2] = false;
            
            int extract = 0;//rec the extracted amount
            string[] Name = new string[8];//rec the name of ATM's from routes
            int[] Tn = new int[8];//Time of the route
            int n = 0;
            bool valabilityC1= true;//Valability of Cards
            bool valabilityC2 = false;
            bool valabilityC3 = true;

            Console.Write("The Route is from point A to:");

            //this function extract the  amount from all valid cards and go to next ATM.
            while (n < 8 )
            {
                int amountCARD1 = 3000;
                int amountCARD2 = 25000;
                int amountCARD3 = 20000;

                int CARD1max = 4000;
                int CARD2max = 3000;
                int CARD3max = 4500;
                int ST = 7000;
                Console.WriteLine("");
              


                for (int m = 0; m <= 4; m++)
                {
                    if (ST > 0 && isOpen[n,m])
                    {
                        int amount = 5000;
                        Tn[n] = Tn[n] + TabTime[n, m];
                        Name[m] = TabName[n, m];
                       
                        Console.Write(">>" + TabName[n, m]);


                        if (valabilityC1 == true)
                        {
                            if (amount >= CARD1max)//5000>=4000 //5000>=4000
                            {
                                if (amountCARD1 > 0)//3000//0>0
                                {
                                    if (amountCARD1 < CARD1max)//3000<4000
                                    {
                                        if (ST > amountCARD1)//7000>3000
                                        {
                                            ST = ST - amountCARD1;//ST = 4000 
                                            extract = amountCARD1;//extract = 3000
                                            amount = amount - extract;//2000
                                            amountCARD1 = 0;

                                        }
                                        else
                                        {
                                            amountCARD1 = amountCARD1 - ST;
                                            ST = 0;
                                        }
                                    }
                                }

                            }
                        }

                        if (valabilityC2 == true && ST > 0)
                        {
                            if (amount >= CARD2max)
                            {
                                if (amountCARD1 > 0)
                                {
                                    if (amountCARD1 < CARD2max)
                                    {
                                        if (ST > amountCARD2)
                                        {
                                            ST = ST - amountCARD2; 
                                            extract = amountCARD2;
                                            amountCARD2 = 0;

                                        }
                                        else
                                        {
                                            amountCARD2 = amountCARD2 - ST;
                                            ST = 0;
                                        }
                                    }
                                }

                            }
                        }

                        if (valabilityC3 == true && ST > 0)
                        {
                            if (amount >= CARD3max)//2000>=4500//5000>=4500
                            {
                                if (amountCARD3 > 0)//18000
                                {
                                    if (amountCARD3 > CARD3max)//18000<4500
                                    {
                                        if (ST > amountCARD3)
                                        {
                                            ST = ST - amountCARD3;
                                            extract = amountCARD3;
                                            amountCARD3 = 0;

                                        }
                                        else
                                        {
                                            amountCARD3 = amountCARD3 - ST;
                                            ST = 0;
                                        }
                                    }
                                    else
                                    {
                                        ST = 0;
                                    }
                                }
                                else
                                    ST = ST - amount;
                                amountCARD3 = amountCARD3 - amount;
                                extract = extract + amount;
                                amount = 0;
                            }
                            else
                            {
                                ST = ST - amount;//ST = 4000 - 2000 = 2000 
                                amountCARD3 = amountCARD3 - amount;//20000 - 2000 = 18000 
                                extract = extract + amount;
                                amount = 0;
                            }
                        }






                    }
                    

                }
                n++;
            }

            Console.WriteLine("Time  required:" + Tn[0] + "min");
            //Output: The rout is from point A to ATM1 to ATM2
        } 
    }
}


using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Collections;

namespace Mastermind
{
    class RandomNumber
    {
        public Dictionary<int,int> GenerateRandom()
        {
            Random random = new Random();
            Dictionary<int, int> randNumber = new Dictionary<int, int>();
            

            for (int i = 0; i < 4; i++)
            {
                randNumber.Add(i,random.Next(1, 6));
            }
            return randNumber;
        }

        public int Compare(Dictionary<int, int> l1, Dictionary<int, int> l2)
        {

            string s1 = string.Join("", l1.Values.ToArray());
            string s2 = string.Join("", l2.Values.ToArray());
            List<int> temp = new List<int>();

            Dictionary<int, int> d = new Dictionary<int, int>();
            Dictionary<int, int> d1 = new Dictionary<int, int>();
            Dictionary<int, int> temp1 = new Dictionary<int, int>();




            Console.WriteLine("Here is your input: " + s1);
            Console.WriteLine("Here is the random number: " + s2);

            if (String.Compare(s1, s2) == 0)
            {
                Console.WriteLine("YOU GOT IT");
                return 0;
            }

            IEnumerable<KeyValuePair<int, int>> x = l1.Except(l2);

            foreach (KeyValuePair<int, int> v in x)
            {
                if (!l2.ContainsValue(v.Value))
                    continue;
                Console.WriteLine("- " + v.Value.ToString());
                temp1.Add(v.Key,v.Value);
            }

                

            x = l1.Except(temp1);

            foreach (KeyValuePair<int, int> v in x)
            {
                if (!l2.ContainsValue(v.Value))
                    continue;
                Console.WriteLine("+ " + v.Value.ToString());
                //temp1.Add(v.Key, v.Value);
            }







            return 1;

            
        }

        public Boolean ValidateParams(string input, out Dictionary<int,int> list)
        {
            int result = 0;
            list  = new Dictionary<int, int>();
            String inputStr = String.Empty;

            try
            {
                if (input.Length != 4)
                {
                    throw new Exception("Please enter a number formed of  4 digits, each between 1 and 6.");
                }
                else if (!Int32.TryParse(input, out result))
                {
                    throw new Exception("You need to input a number");

                }


                for (int i = 0; i < 4; i++)
                {
                    int temp = Int32.Parse(input[i].ToString());
                    if ((temp < 1) || (temp > 6))
                    {
                        throw new Exception("Each digit needs to be between 1 and 6");
                    }
                    else
                        list.Add(i,temp);
                }

                if (list.Count == 4)
                {
                    return true;

                }
                else
                    return false;

            }
            catch (Exception e)
            {
                throw new Exception(e.Message.ToString());
                
            }


        }
    }
    class Program
    {
        static void Main(string[] args)
        {
            string inputStr = String.Empty;
            Dictionary<int, int> input = new Dictionary<int, int>();
            Dictionary<int, int> randNbr = new Dictionary<int, int>();
            RandomNumber myNumber = new RandomNumber();
            Boolean check = false;


            int result = 0;
            try
            {
                randNbr = myNumber.GenerateRandom();
                Console.WriteLine("Please input a number with 4 digits, each digit between 1 and 6; You have 10 tries to guess a random number. ");
                for (int i = 0; i < 10; i++)
                {
                    string inputTest = Console.ReadLine().Trim();
                    //Console.WriteLine(inputTest);
                    check = myNumber.ValidateParams(inputTest, out input);
                    if (!check)
                        throw new Exception("Check your input...");
                    result = myNumber.Compare(input, randNbr);
                    if (result == 0)
                        break;


                }
                              
                
                if (result == 1)
                    Console.WriteLine("Better luck next time ...");
                Console.ReadLine();

             }
            catch ( Exception ex)
            {
                Console.WriteLine(ex.Message.ToString());
                Console.ReadLine();
            }

            
            
        }
    }
}

# SKIPLIST C#
Data structures Project Skip list in c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.IO;

namespace mySkiplist1
{
    class Program
    {
        static void Main(string[] args)
        {
            Skiplist s = new Skiplist(5);
           
            Random r = new Random();
            int a = r.Next(0, 5);// coin flipped to determine levels
            foreach (string i in File.ReadAllLines("C:\\Users\\samma\\Desktop\\sam1.txt"))
            {
                s.Insert(Convert.ToInt32(i),a);// each node isnerted with varibale levels
                s.Insert(Convert.ToInt32(i), a);
            }
           
            s.print();
            s.Remove(2);
            s.print();
           
            Console.ReadKey();
        }
    }
    class Nodes
    {
        public Nodes [] Next;// refrence to next array of nodes
        public int values;
        public Nodes(int level,int val)
        {
            this.values = val;
            this.Next = new Nodes[level];
        }

    }
    class Skiplist
    {
        // a header is set of an array consisting 0 values and levels determined at the constructor
       public Nodes header;
        public int levels;
        public Skiplist(int level)
        {
             header = new Nodes(level,0);
            levels = level;
        }
        // values insert starting from top header values to reach each level the value of isnerted node level is directly asigmetnand the no of interation in the
        //next array links all the array nodes to each invdiual node in array

        public void Insert(int value,int level)
        {
            Nodes n = new Nodes(level,value);
            Nodes current = header;
           for(int i=0;i<level;i++)
            {
                if (current.Next[i] == null)// if list in empty
                {
                    current.Next[i] = n;
                }
                while (current.values < value)//if insetrted value is greated than the next value of node
                {
                    if (current.Next[i] == null || current.Next[i].values > value)
                    {
                        Nodes temp = current.Next[i];
                        current.Next[i] = n;
                        n.Next[i] = temp;
                    }
                    else
                    {

                        current = current.Next[i];
                    }

                }
                while (current.values > value)// if inserted cvalue is less the next value of node
                {
                    Nodes temp1 = current.Next[i];
                    current.Next[i] = n;
                    n.Next[i] = temp1;

                }

                


            }
        }
        public Nodes Search(int value)
        {
            // search occurs started from the top values of array 
            Nodes cur = header; 
            for (int i = levels - 1; i >= 0; i--)
            {
                for (; cur.Next[i] != null; cur = cur.Next[i])//nodes skipped if next pointer to node is null and simultaneously goes down level
                {
                    if (cur.Next[i].values > value)
                        break;
                    if (cur.Next[i].values == value)
                        return cur.Next[i];
                }
            }
            return null;
        }
        public bool Remove(int value)
        {
            Nodes cur = header;

            bool found = false;
            for (int i = levels - 1; i >= 0; i--)
            {
                for (; cur.Next[i] != null; cur = cur.Next[i])// same priciple as search but delete node links are remove with the next and previos array 
                {
                    if (cur.Next[i].values == value)
                    {
                        found = true;
                        cur.Next[i] = cur.Next[i].Next[i];
                        break;
                    }

                    if (cur.Next[i].values > value) break;
                }
            }

            return found;
        }
        public void print()
        {
            using (StreamWriter sam = new StreamWriter("C:\\Users\\samma\\Desktop\\sam.txt"))
            {
                Nodes cur = header;
                while (cur.Next[0] != null)
                {
                    cur = cur.Next[0];
                    Console.WriteLine(cur.values);
                    sam.Write(cur.values);
                }
            }
            }
           
        }
    }


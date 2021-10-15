# kuliah
game rpg (prototype)

using System;
using System.Collections.Generic;

namespace tugas_permainan_rpg
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Welcome to my Adventure game");
            Console.WriteLine("whay is your name?");
            // inialisasi player
            Novice newPlayer = new Novice();
            KNIGHT name_knight = new KNIGHT();

            // inialisasi enemy
            enemy butterfly = new enemy("butterfly");
            enemy Slime = new enemy("Slime");
            enemy Goblin = new enemy("Goblin");
            Boss DemonLord = new Boss("Demon Lord");

            {
                List<string> enemyName = new List<string> {
                "Butterfly",
                "Slime",
                "Goblin",
                "Demon Lord[Boss]",
            };


                newPlayer.Name = Console.ReadLine();
                Console.WriteLine("hi" + newPlayer.Name + " ready for new adventure?[y/n]");
                string bReady = Console.ReadLine();
                if (bReady == "y")
                {
                    Console.WriteLine(newPlayer.Name + " is entering the world");
                    Console.WriteLine(newPlayer.Name + " is encountering " + butterfly);
                    Console.WriteLine(butterfly.Name + " is attacking you !");
                    Console.WriteLine("Choose your action :");
                    Console.WriteLine("1. Single Attack");
                    Console.WriteLine("2. Swing Attack");
                    Console.WriteLine("3. Defend");
                    Console.WriteLine("4. Run away");
                    while (!newPlayer.isdead && !butterfly.isdead)
                    {
                        string playerAction = Console.ReadLine();
                        switch (playerAction)
                        {
                            case "1":
                                Console.WriteLine(newPlayer.Name + " is doing single attack");
                                butterfly.gethit(butterfly.attackPower);
                                Console.WriteLine("player health : " + newPlayer.health + " | enemy health : " + butterfly.health);
                                break;
                            case "2":
                                newPlayer.SpecialSkill();
                                newPlayer.exp += 0.9f;
                                butterfly.gethit(newPlayer.attackPower);
                                Console.WriteLine("player health : " + newPlayer.health + " | enemy health :" + butterfly.health);
                                break;
                            case "3":
                                newPlayer.rest();
                                Console.WriteLine("energy is being restored");
                                butterfly.Attack();
                                newPlayer.gethit(butterfly.attackPower);
                                break;
                            case "4":
                                Console.WriteLine(newPlayer + " attempt to run away...");
                                break;

                        }

                    }
                    Console.WriteLine(newPlayer.Name + "get" + newPlayer.exp + "experience");



                }
            }
        }
        class Novice
        {
            public string Name { get; set; }

            public int attackPower { get; set; }

            public int health { get; set; }
            public int energy { get; set; }
            public float exp { get; set; }
            public bool isdead { get; set; }
            Random rnd = new Random();

            public Novice()
            {
                health = 100;
                energy = 0;
                attackPower = 1;
                isdead = false;
                exp = 0f;
                Name = "newbie";


            }
            public virtual void SpecialSkill()
            {
                if (energy > 0)
                {
                    Console.WriteLine("SWINGGG ATTACKK!!!");
                    attackPower += rnd.Next(3, 11);
                    energy--;
                }

            }

            public void rest()
            {
                energy++;
                attackPower = 1;
            }

            public void gethit(int hitvalue)
            {
                Console.WriteLine(Name + " get hit by" + hitvalue);
                health -= hitvalue;

            }

            private void die()
            {
                Console.WriteLine(Name + " is dead , game over ");
                isdead = true;
            }
        }

        // class KNIGHT

        class KNIGHT : Novice
        {
            public KNIGHT()
            {
                Name = "knight name";
                health = 200;
                energy = 0;
                attackPower = 4;
                isdead = false;
                exp = 0f;

            }
            public override void SpecialSkill()
            {
                Console.WriteLine(Name + " use Bash ,  enemy stun for 2 turn ");

            }
        }



        // enemy tingkat
        class enemy
        {
            public bool isdead { get; set; }
            public string Name { get; set; }

            public int attackPower { get; set; }

            public int health { get; set; }
            Random rnd = new Random();


            public enemy(string name)
            {
                Name = name;
                health = 25;
            }

            public void Attack()
            {
                attackPower = attackPower + rnd.Next(1, 4);
            }

            public void gethit(int hitvalue)
            {
                Console.WriteLine(Name + " get hit by" + hitvalue);
                health -= hitvalue;

                if (health <= 0)
                {
                    health = 0;
                    die();
                }

            }
            private void die()
            {
                Console.WriteLine(Name + " is dead");
                isdead = true;
            }
        }


        // ini adalah stat character boss
        class Boss
        {
            public bool isdead { get; set; }
            public string Name { get; set; }

            public int attackPower { get; set; }

            public int energy { get; set; }


            public int health { get; set; }
            Random rnd = new Random();


            public Boss(string name)
            {
                Name = name;
                health = 1000;

            }

            public void Attack(int target_attackpower)
            {
                attackPower = 3 * target_attackpower;
            }



            public void demolished(int target_health)
            {
                Console.WriteLine(Name + " use special skill , WATCH OUTTT!!!!");
                int instant_chance = rnd.Next(1, 11);
                if (instant_chance == 1)
                {
                    attackPower = target_health;
                }
                else { attackPower = Convert.ToInt32(target_health * 0.33); }

            }



        }


    }
}

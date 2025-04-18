# TextRpg-
스파르타 유니티 내일배움캠프 C#TextRpg만들기
using System.Xml.Linq;
using static ConsoleApp6.Program;

namespace ConsoleApp6
{
    internal class Program
    {
        
        private bool IsEquipped;
        private int Atk;
        private int Def;
        private string Description;
        private static int keyInput;
        private static List<Item> items;
        private static Item[] _items;
        private static int choice;

        public void PrintItemStatDescription(bool withNumber = false, int idx = 0, bool showPrice = false)
        {
            if (withNumber)
            {
                Console.Write("{0} ", idx);
            }
            if (IsEquipped)
            {
                Console.Write("[");
                Console.Write("E");
                Console.Write("]");
                Console.Write(PadRightForMixedText(Player.name, 9));
            }
            Console.Write(PadRightForMixedText(Player.name, 12));

            if (Atk != 0) Console.Write($"Atk {(Atk >= 0 ? " + " : "")}{Atk}");
            if (Def != 0) Console.Write($"Def {(Def >= 0 ? " + " : "")}{Def}");

            Console.Write(Description);
        }

        public static int GetPrintableLength(string str)
        {
            int length = 0;
            foreach (char c in str)
            {
                if (char.GetUnicodeCategory(c) == System.Globalization.UnicodeCategory.OtherLetter)
                {
                    length += 2;
                }
                else
                {
                    length += 1;
                }
            }
            return length;
        }
        public static string PadRightForMixedText(string str, int totalLength)
        {
            int currentLength = GetPrintableLength(str);
            int padding = totalLength - currentLength;
            return str.PadRight(str.Length + padding);
        }

        public static void GameDateSetting()
        {
            _items = new Item[6];
            AddItem(new Item(" 구리 갑옷 ", "방어력 +2, 구리로 만들어진 기본갑옷.", 0, 2, 500));
            AddItem(new Item(" 은 갑옷 ", "방어력 +4, 은으로 만든 갑옥.", 0, 6, 1000));
            AddItem(new Item(" 황금 갑옷 ", "방어력 +6, 금으로 만든 갑옷.", 0, 10, 1500));
            AddItem(new Item(" 나무 목검 ", "공격력 +2, 제일 기본 나무로 만들어진 검.", 2, 0, 500));
            AddItem(new Item(" 단검 ", "공격력 +4, 짧은 검.", 5, 0, 1000));
            AddItem(new Item(" 철검 ", "공격력 +6, 철로 만들어진 기본검.", 7, 0, 1500));
        }

        public class Player()
        {


            public static string Level { get; set; } = "01";
            public static string name { get; set; } = "김희주";
            public static int Atk { get; set; } = 10;
            public static int Def { get; set; } = 5;
            public static int Hp { get; set; } = 100;
            public static string Job { get; set; } = "전사";
            public static int Gold { get; set; } = 2500;
        }
        
        static void Main(string[] args)
        {
            Console.WriteLine("스파르타 던전에 오신 여러분 환영합니다.");
            Console.WriteLine("스파르타 마을에 오신 여러분 환영합니다.");
            Console.WriteLine("이곳에서 던전으로 들어가기전 활동을 할 수 있습니다. \n");

            Satge satge = new Satge();
            satge.getSelect();
        }
        static void SetSelect()
        {
            while (true)
            {
                if (int.TryParse(Console.ReadLine(), out int userInput))
                {
                    if (userInput == 0)
                    {
                        Console.Clear();
                        Satge satge = new Satge();
                        satge.getSelect();
                        break;
                    }
                    else
                    {
                        Console.WriteLine("잘못된 입력입니다.");
                    }
                }
            }
        }
    
        public class Satge
        {
            public void getSelect()
            {
                while (true)
                {
                    Console.WriteLine("1.상태보기");
                    Console.WriteLine("2.인벤토리");
                    Console.WriteLine("3.상점\n");
                    Console.WriteLine("원하시는 행동을 입력해주세요\n");

                    if (int.TryParse(Console.ReadLine(), out int selectInput))

                    {
                        switch (selectInput)
                        {
                            case 1:
                                PlayerInfo();
                                break;
                            case 2:
                                Inventory();
                                break;
                            case 3:
                                Store();
                                break;
                        }
                        if (selectInput >= 1 && selectInput <= 3)
                            break;
                    }
                    else
                    {
                        Console.WriteLine("잘못된 입력입니다.");
                    }
                }
            }
         }
        static void PlayerInfo()
        {
            Console.Clear();
            Console.WriteLine("상태 보기");
            Console.WriteLine("캐릭터의 정보가 표시됩니다.\n");
            Console.WriteLine($" Lv. {Player.Level}");
            Console.WriteLine($" 이름 : {Player.name} ({Player.Job})");
            Console.WriteLine($" 공격력 : {Player.Atk} ");
            Console.WriteLine($" 방어력 : {Player.Def}");
            Console.WriteLine($" 체력 : {Player.Hp}");
            Console.WriteLine($" Gold : {Player.Gold} G");
            Console.WriteLine(" 원하시는 행동을 입력해주세요.\n");
            Console.WriteLine(" 0. 나가기\n");
            SetSelect();
        }
        static void Inventory()
        {
            Console.Clear();
            Console.WriteLine("인벤토리");
            Console.WriteLine("보유 중인 아이템을 관리할 수 있습니다.\n");
            Console.WriteLine(" 원하시는 행동을 입력해주세요.\n");
            Console.WriteLine("[아이템 목록]\n");
            for (int i = 0; i < Item.ItemCnt; i++)
            {
                if (_items[i].IsPurchased)
                {
                    _items[i].PrintItemStatDescription(true, i + 1, false, true);
                }
            }
            Console.WriteLine(" 1. 장착 관리");
            Console.WriteLine(" 0. 나가기\n");
            while (true)
            {
                if (int.TryParse(Console.ReadLine(), out int InventoryInput))
                {
                    switch (InventoryInput)
                    {
                        case 0:
                            SetSelect();
                            break;
                        case 1:
                            Equip();
                            break;
                        default:
                            Console.WriteLine("잘못된 입력입니다.\n");
                            continue;
                    }
                }
            }
        }
        
         static void AddItem(Item item)
        {
            if (Item.ItemCnt == 6) return;
            _items[Item.ItemCnt] = item;
            Item.ItemCnt++;
        }
        static void Store()
        {
            Console.Clear();
            Console.WriteLine("상점");
            Console.WriteLine("필요한 아이템을 얻을 수 있는 상점입니다.\n");
            Console.WriteLine("[보유 골드]");
            Console.WriteLine($" Gold : {Player.Gold} G\n");
            List<Item> items = new List<Item>
            {
                new Item(" 구리 갑옷 ", "방어력 +2, 구리로 만들어진 기본갑옷.", 0 , 2, 500),
                new Item(" 은 갑옷 ", "방어력 +4, 은으로 만든 갑옥.", 0, 6, 1000),
                new Item(" 황금 갑옷 ", "방어력 +6, 금으로 만든 갑옷.", 0, 10, 1500),
                new Item(" 나무 목검 ", "공격력 +2, 제일 기본 나무로 만들어진 검.", 2, 0, 500),
                new Item(" 단검 ", "공격력 +4, 짧은 검.", 5, 0, 1000),
                new Item(" 철검 ", "공격력 +6, 철로 만들어진 기본검.", 7, 0, 1500)
            };

            Console.WriteLine("[아이템 목록]\n");
            for (int i = 0; i < items.Count; i++)
            {
                Console.WriteLine($"{i + 1}. {items[i].ItemName} | {items[i].ToolTip} | {items[i].Price}G{(items[i].Purchase ? " - 구매 완료" : "")}");
            }

            foreach (var item in items)
            {
                item.Purchase = false;
            }
            Console.WriteLine(" 원하시는 행동을 입력해주세요.\n");
            Console.WriteLine(" 1. 아이템 구매");
            Console.WriteLine(" 0. 나가기\n");
            while (true)
            {
                if (int.TryParse(Console.ReadLine(), out int storeInput))
                {
                    switch (storeInput)
                    {
                        case 0:
                            SetSelect();
                            break;
                        case 1:
                            BuyItem(items);
                            break;
                        default:
                            Console.WriteLine("잘못된 입력입니다.\n");
                            continue;
                    }
                }
            }
        }
        static void BuyItem(List<Item> items)
        {
            while (true)
            {
                Console.Clear();

                Console.Clear();
                Console.WriteLine("[ 상점아이템 구매 ]\n보유하신 골드로 아이템을 구매 할 수 있습니다.\n");
                Console.WriteLine("[보유 골드]");
                Console.WriteLine($"{Player.Gold} G\n");

                Console.WriteLine("[ 아이템 목록 ]");
                for (int i = 0; i < items.Count; i++)
                {
                    Console.WriteLine($"{i + 1}. {items[i].ItemName} | {items[i].ToolTip} | {items[i].Price}G{(items[i].Purchase ? " - 구매 완료" : "")}");
                }
                Console.WriteLine("\n구매하실 아이템의 번호를 입력하세요. 0은 뒤로 가기 입니다.");

                if (int.TryParse(Console.ReadLine(), out int itemInput) && itemInput >= 0 && itemInput <= items.Count)
                {
                    if (itemInput == 0)
                    {
                        Store();
                        break;
                    }
                    else
                    {
                        Item buyItems = items[itemInput - 1];

                        if (buyItems.Purchase)
                        {
                            Console.WriteLine("이미 구매한 아이템입니다.\n");
                        }
                        else if (Player.Gold >= buyItems.Price)
                        {
                            Console.WriteLine("구매를 완료했습니다.\n");
                            Player.Gold -= buyItems.Price;
                            buyItems.Purchase = true;
                            buyItems.IsPurchased = true;
                        }
                        else
                        {
                            Console.WriteLine("골드가 부족합니다.\n");
                        }
                    }
                    Console.WriteLine("\n아무 키나 누르면 돌아갑니다. ");
                    Console.ReadKey();
                }
                else
                {
                    Console.WriteLine("잘못된 입력입니다.");
                    Console.ReadKey();
                }
            }
        }
        public static void Equip()
        {
            Console.Clear();
            Console.WriteLine("인벤토리 - 장착 관리");
            Console.WriteLine("보유 중인 아이템을 관리할 수 있습니다.\n");
            Console.WriteLine("[아이템 목록]\n");
            for (int i = 0; i < Item.ItemCnt; i++)
            {
                if (_items[i].IsPurchased)
                {
                    _items[i].withNumber(true, i + 1, false);
                    Console.WriteLine("아이템을 장착했습니다.");
                }
            }
            Console.WriteLine("0. 나가기");
            while (true)
            {
                if (int.TryParse(Console.ReadLine(), out int EquipInput))
                {
                    switch (EquipInput)
                    {
                        case 0:
                            Inventory();
                            break;
                        default:
                            ToggleEquipStatus(keyInput - 1);
                            continue;
                    }
                }
            }
        }

        private static void ToggleEquipStatus(int idx)
        {
            _items[idx].IsEquipped = !_items[idx].IsEquipped;
        }
        public class Item
        {
            public string ItemName { get; set; }
            public string ToolTip { get; set; }
            public int Attack { get; set; }
            public int Defense { get; set; }
            public int Price { get; set; }
            public bool Purchase { get; set; }
            public bool IsEquipped { get; set; }
            public bool IsPurchased { get; set; }
            public string Description { get; }
            public object Type { get; internal set; }

            public static int ItemCnt = 0;
            internal object Player;

            public Item(string itemName, string toolTip, int attack, int defense, int price, bool isEquipped = false, bool isPurchased = false)
            {
                ItemName = itemName;
                ToolTip = toolTip;
                Attack = attack;
                Defense = defense;
                Price = price;
                Purchase = false;
                IsEquipped = isEquipped;
                IsPurchased = isPurchased;
            }

            internal void PrintItemStatDescription(bool v1, int v2, bool v3, bool v4)
            {
                throw new NotImplementedException();
            }
            internal void withNumber(bool v1, int v2, bool v3)
            {
                throw new NotImplementedException();
            }
        }
    }
}

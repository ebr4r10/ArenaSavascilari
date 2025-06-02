# ArenaSavascilari

    public class Karakter
    {
        public string Ad { get; set; } //karakterin olması gereken özelliklerini ekledim burada
        public int Can { get; set; }  
        public int Guc { get; set; }
        public int Mana { get; set; }
      
        public static int ToplamSaldiriSayisi = 0;  // tüm karakterlerin yaptığı saldırıları tutyor

        public Karakter() { }
        public void BilgileriGoster() // hiç bir şey başlamadan başlangıçta sahip oldugu isim, can, güç, mana özelliklerini ekrana yazdırmak için
        {
            Console.WriteLine($"{Ad} - Can: {Can}, Güç: {Guc}, Mana: {Mana}");
        }
        
        public void HasarAl(int miktar)// Karakter hasar aldığında canını azaltır ve kalan canı yazdırır
        {
            Can = Can-miktar;
            if (Can < 0)
                Can = 0;
            Console.WriteLine($"{Ad} {miktar} hasar aldı! Kalan Can: {Can}");
        }
    }
    public class Oyuncu : Karakter //burada oyuncu sınıfı karakter sınıfından miras alıyor
    {
        public Oyuncu(string ad, int can, int guc, int mana)
        {
            Ad = ad;
            Can = can;
            Guc = guc;
            Mana = mana;
        }
        
        public void Saldir(Dusman saldir)// Oyuncu düşmana saldırır, düşman hasar alır ve toplam saldırı sayısı artar
        {
            saldir.HasarAl(this.Guc);
            Karakter.ToplamSaldiriSayisi++;
            Console.WriteLine($"{Ad}, {saldir.Ad} adlı düşmana {Guc} hasar verdi!");
        }
        
        public void ManaYenile() // Düşman oyuncuya saldırır, oyuncu hasar alır ve toplam saldırı sayısı artar
        {
            Mana += 10;
        }

    }
    public class Dusman : Karakter // düşman sınıfı karakter sınıfından miras alıyor
    {
        public Dusman(string ad, int can, int guc, int mana)
        {
            Ad = ad;
            Can = can;
            Guc = guc;
            Mana = mana;
        }

        public void Saldir(Oyuncu saldr) // Düşman oyuncuya saldırır, oyuncu hasar alır ve toplam saldırı sayısı artar
        {
            saldr.HasarAl(this.Guc);
            Karakter.ToplamSaldiriSayisi++; 
            Console.WriteLine($"{Ad}, {saldr.Ad} adlı oyuncuya {Guc} hasar verdi!");
        }
        internal class Program
        {
            static void Main(string[] args)
            {
                // Oyuncu ve düşman nesneleri oluşturuluyor
                Oyuncu oyuncu = new Oyuncu("HERO", 100, 20, 30); //toplam başlangıçta sahip oldukları özellikler
                Dusman goblin = new Dusman("GOBLİN", 100, 10, 0); //toplam başlangıçta sahip oldukları özellikler

                Console.WriteLine("     savaş başladı!    ");
                // Başlangıç bilgileri ekrana yazdırılıyor
                oyuncu.BilgileriGoster();
                goblin.BilgileriGoster();

                int turSayisi = 0;

                while (oyuncu.Can > 0 && goblin.Can > 0) // Oyuncu ve düşmanın canı sıfırdan büyük olduğu sürece döngü devam eder
                {
                    if (turSayisi % 2 == 0)// Çift turlarda oyuncu hamlesi yapar
                    {
                        Console.WriteLine("1. saldır");
                        Console.WriteLine("2. mana yenile");
                        Console.Write("Seçim: ");
                        string secim = Console.ReadLine();

                        if (secim == "1")// Oyuncu saldırı seçerse gobline saldırır
                        { 
                            oyuncu.Saldir(goblin);
                        }
                        else if (secim == "2")// Mana yenileme seçeneği
                        {
                            oyuncu.ManaYenile();
                            Console.WriteLine("mana yenilendi");
                        }
                    }
                    else
                    {
                        goblin.Saldir(oyuncu);
                    }

                    // Her tur sonunda karakterlerin güncel bilgileri gösterilir
                    goblin.BilgileriGoster();
                    oyuncu.BilgileriGoster();

                    turSayisi++;// Tur sayısı artırılır
                }

                if (oyuncu.Can <= 0)
                    Console.WriteLine("        -  kaybettin hahaha -"); //duruma göre ekrana yazdırılan sonuçlar
                else
                    Console.WriteLine( "     - helal olsun kazandın - ");

            }
      


        }
    }


    


   ![Ekran görüntüsü 2025-06-02 160804](https://github.com/user-attachments/assets/13e998c8-e10a-4a95-8376-acd328009a44)

    
  ![Ekran görüntüsü 2025-06-02 170034](https://github.com/user-attachments/assets/d47d072e-d78d-42cb-b088-ac708384b4ff)
  


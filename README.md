# ArenaSavascilari

 public class Karakter
 {
     public string Ad { get; set; }
     public int Can { get; set; }  
     public int Guc { get; set; }
     public int Mana { get; set; }
   
     public static int ToplamSaldiriSayisi = 0;  // Tüm karakterlerin toplam yaptığı saldırı sayısını tutar

     public Karakter() { }
     public void BilgileriGoster()
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
 public class Oyuncu : Karakter
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
 public class Dusman : Karakter
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
             Oyuncu oyuncu = new Oyuncu("HERO", 100, 20, 30);
             Dusman goblin = new Dusman("GOBLİN", 100, 10, 0);

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
                 Console.WriteLine("        - ezik kaybettin hahaha -");
             else
                 Console.WriteLine( "     - helal olsun kazandın - ");

         }

     }
 }

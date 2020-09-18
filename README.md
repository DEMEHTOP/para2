using System;
using System.Security.Cryptography;
using System.Text;
 
namespace Vitomsky_md5_1
{
    public static class ArrayExtension
    {
        public static void Display(this byte[] bytes)
        {
            Console.WriteLine("Displaying byte array: ");
            foreach (var @byte in bytes)
            {
                Console.Write($"{@byte} ");
            }
            Console.WriteLine();
        }
 
        public static bool CompareHashes(this byte[] original, byte[] compareTo)
        {
            if (original.Length != compareTo.Length)
            {
                Console.WriteLine("Длины хешей не равны");
                return false;
            }
 
            Console.WriteLine("Длины хешей равны");
            
            for (var index = 0; index < original.Length; index++)
            {
                if (original[index] != compareTo[index])
                {
                    return false;
                }
            }
 
            return true;
        }
 
        public static int ComputeOneInByte(this int num)
        {
            int result = 0;
            while (num != 0)
            {
                var one = num & 1;
 
                result = result + one;
                num = num >> 1;
            }
 
            return result;
        }
 
        public static void DisplayCompareInformation(this byte[] original, byte[] compareTo)
        {
            var equal = 0;
            var diffrent = 0;
            
            var equalBits = 0;
            var differentBits = 0;
 
            if (original.Length != compareTo.Length)
            {
                Console.WriteLine("длины не совпадают");
            }
            Console.WriteLine("Длины совпадают");
 
            Console.WriteLine($"Длины хешей = {original.Length}");
            
            for (int index = 0; index < original.Length; index++)
            {
                if (original[index] != compareTo[index])
                {
                    diffrent++;
                }
                else
                {
                    equal++;
                }
            }
            
            Console.WriteLine($"Совпадающих байт {equal} ; Разных байт : {diffrent}");
 
            for (var index = 0; index < original.Length; index++)
            {
                var xorResult = original[index] ^ compareTo[index];
                
                Console.WriteLine($"Result of xor : {xorResult}");
 
                var onesResult = xorResult.ComputeOneInByte();
                equalBits += onesResult;
                differentBits += (8 - onesResult);
            }
            
            Console.WriteLine($"Совпадающие биты : {equalBits} ; Разные биты {differentBits}");
        }
    }
    
    class Program
    {
 
        private static byte[] ComputeMD5(string text)
        {
            var textBytes = Encoding.ASCII.GetBytes(text);
            using var cryptoProvider = new MD5CryptoServiceProvider();
            return cryptoProvider.ComputeHash(textBytes);
        }
 
        private static void Task2(string original)
        {
            int position = 5;
 
            var bufferText = original.ToCharArray();
            bufferText[position]++;
 
            var editedText = new StringBuilder().Append(bufferText).ToString();
 
            Console.WriteLine($"Original : {original} ; Edited : {editedText}");
            
            var hash1 = ComputeMD5(original);
            var hash2 = ComputeMD5(editedText);
            
            hash1.Display();
            hash2.Display();
            
            hash1.DisplayCompareInformation(hash2);
        }
 
        static void Main(string[] args)
        {
            var someTestText1 = "Hello, world!";
            var someTestText2 = "Some trash text";
 
            var someTestTexts = new []
            {
                "Hello, world!",
                "Some trash text",
                "HHHJHJKHfdf",
                "KKKHGGFGH"
            };
            
            var result1 = ComputeMD5(someTestText1);
            var result2 = ComputeMD5(someTestText2);
            
            result2.Display();
            result1.Display();
 
            var compareResult1 = result1.CompareHashes(result2);
 
            if (compareResult1)
            {
                Console.WriteLine("Хешы равны");
            }
            else
            {
                Console.WriteLine("Хешы не равны");
            }
            
            Console.WriteLine("Вторая часть ПЗ");
 
            foreach (var text in someTestTexts)
            {
                Console.WriteLine("_____________________________");
                Task2(text);
                Console.WriteLine("_____________________________");
            }
        }
    }
}

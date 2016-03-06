---
title: Csharp分割字符串
date: 2016-03-05 16:34:19
---
命名空间：System.String.Split

程序集：mscorlib( mscorlib.dll)

 

简单实例：

	string before = "12,50,30";

	string[] after =before.Split(new char[]{','}); 

	//结果为 after[0] = 12;  after[1] = 50; after[2] = 30;

#1.正则表达
如果字符串是混合模式，即同时含有不同的类型，可以使用以下的方法分割他们的元素。
	using System;
	using System.Text.RegularExpressions;
	
	public class Example
	{
	   public static void Main()
	   {
	      String[] expressions = { "16 + 21", "31 * 3", "28 / 3",
	                               "42 - 18", "12 * 7",
	                               "2, 4, 6, 8" };
	      String pattern = @"(\d+)\s+([-+*/])\s+(\d+)";
	      foreach (var expression in expressions)
	         foreach (Match m in Regex.Matches(expression, pattern)) {
	            int value1 = Int32.Parse(m.Groups[1].Value);
	            int value2 = Int32.Parse(m.Groups[3].Value);
	            switch (m.Groups[2].Value)
	            {
	               case "+":
	                  Console.WriteLine("{0} = {1}", m.Value, value1 + value2);
	                  break;
	               case "-":
	                  Console.WriteLine("{0} = {1}", m.Value, value1 - value2);
	                  break;
	               case "*":
	                  Console.WriteLine("{0} = {1}", m.Value, value1 * value2);
	                  break;
	               case "/":
	                  Console.WriteLine("{0} = {1:N2}", m.Value, value1 / value2);
	                  break;
	            }
	         }
	   }
	}
	// The example displays the following output:
	//       16 + 21 = 37
	//       31 * 3 = 93
	//       28 / 3 = 9.33
	//       42 - 18 = 24
	//       12 * 7 = 84

> \s-
> 
> Match a whitespace character followed by a hyphen.
> 
> \s?
> 
> Match zero or one whitespace character.
> 
> [+*]?
> 
> Match zero or one occurrence of either the + or * character.
> 
> \s?
> 
> Match zero or one whitespace character.
> 
> -\s
> 
> Match a hyphen followed by a whitespace character.

	using System;
	using System.Text.RegularExpressions;
	
	public class Example
	{
	   public static void Main()
	   {
	      String input = "[This is captured\ntext.]\n\n[\n" +
	                     "[This is more captured text.]\n]\n" +
	                     "[Some more captured text:\n   Option1" +
	                     "\n   Option2][Terse text.]";
	      String pattern = @"\[([^\[\]]+)\]";
	      int ctr = 0;
	      foreach (Match m in Regex.Matches(input, pattern))
	         Console.WriteLine("{0}: {1}", ++ctr, m.Groups[1].Value);
	   }
	}
	// The example displays the following output:
	//       1: This is captured
	//       text.
	//       2: This is more captured text.
	//       3: Some more captured text:
	//          Option1
	//          Option2
	//       4: Terse text.

> \[
> 
> Match an opening bracket.
> 
> ([^\[\]]+)
> 
> Match any character that is not an opening or a closing bracket one or more times. This is the first capturing group.
> 
> \]
> 
> Match a closing bracket.

#搜索指定的字符

	using System;
	using System.Collections.Generic;
	
	public class Example
	{
	   public static void Main()
	   {
	      String value = "This is the first sentence in a string. " +
	                     "More sentences will follow. For example, " +
	                     "this is the third sentence. This is the " +
	                     "fourth. And this is the fifth and final " +
	                     "sentence.";
	      var sentences = new List<String>();
	      int position = 0;
	      int start = 0;
	      // Extract sentences from the string.
	      do {
	         position = value.IndexOf('.', start);
	         if (position >= 0) {
	            sentences.Add(value.Substring(start, position - start + 1).Trim());
	            start = position + 1;
	         }
	      } while (position > 0);
	
	      // Display the sentences.
	      foreach (var sentence in sentences)
	         Console.WriteLine(sentence);
	   }
	}
	// The example displays the following output:
	//       This is the first sentence in a string.
	//       More sentences will follow.
	//       For example, this is the third sentence.
	//       This is the fourth.
	//       And this is the fifth and final sentence.

> IndexOf , 返回某个特定字符或者字符串第一次出现的位置，which returns the zero-based index of the first occurrence of a character or string in a string instance.
> 
> IndexOfAny , 返回某个或多个特定字符或者字符串第一次出现的位置，which returns the zero-based index in the current string instance of the first occurrence of any character in a character array.
> 
> LastIndexOf ,返回某个特定字符或者字符串最后一次出现的位置 ，which returns the zero-based index of the last occurrence of a character or string in a string instance.
> 
> LastIndexOfAny, which returns a zero-based index in the current string instance of the last occurrence of any character in a character array.
> 
> string.IndexOf/string.LastIndexOf

IndexOf方法用于搜索在一个字符串中，某个特定的字符或者子串第一次出现的位置，该方法区分大小写，并从字符串的首字符开始以0计数。如果字符串中不包含这个字符或子串，则返回-1。

定位字符：
	int IndexOf(char value)
	int IndexOf(char value, int startIndex)
	int IndexOf(char value, int startIndex, int count)

定位子串：
	int IndexOf(string value)
	int IndexOf(string value, int startIndex)
	int IndexOf(string value, int startIndex, int count)

在上述重载形式中，其参数含义如下：
value：待定位的字符或者子串。
startIndex：在总串中开始搜索的其实位置。
count：在总串中从起始位置开始搜索的字符数

例如：

	string str = "那么骄傲少爷的身子跑堂儿的命儿那么骄傲少爷的身子跑堂儿的命儿";
	string str1 = str.IndexOf("百度").ToString();          //返回 -1
	string str2 = str.IndexOf("少爷").ToString();          //返回 4
	string str3 = str.IndexOf("少爷", 10).ToString();     //返回19 说明：这是从第10个字符开始查起。
	string str4 = str.IndexOf("爷", 10, 5).ToString();     //返回 -1
	string str5 = str.IndexOf("爷", 10, 20).ToString();   //返回 20 说明：从第10个字符开始查找，要查找的范围是从第10个字符开始后20个字符，即从第10-30个字符中查找。

同IndexOf类似，LastIndexOf用于搜索在一个字符串中，某个特定的字符或者子串最后一次出现的位置，其方法定义和返回值都与IndexOf相同，不再赘述。

IndexOfAny/LastIndexOfAny
IndexOfAny方法功能同IndexOf类似，区别在于，它可以搜索在一个字符串中，出现在一个字符数组中的任意字符第一次出现的位置。同样，该方法区分大小写，并从字符串的首字符开始以0计数。如果字符串中不包含这个字符或子串，则返回-1。常用的IndexOfAny重载形式有3种：

	int IndexOfAny(char[]anyOf)；
	int IndexOfAny(char[]anyOf, int startIndex)；
	int IndexOfAny(char[]anyOf, int startIndex, int count)。
在上述重载形式中，其参数含义如下：
anyOf：待定位的字符数组，方法将返回这个数组中任意一个字符第一次出现的位置。
startIndex：在原字符串中开始搜索的其实位置。
count：在原字符串中从起始位置开始搜索的字符数。

例如：

	String s = "Hello";
	char[] anyOf = { 'H', 'e', 'l' };
	int i1 = s.IndexOfAny(anyOf);       //返回 0
	int i2 = s.LastIndexOfAny(anyOf);   //返回 3

同IndexOfAny类似，LastIndexOfAny用于搜索在一个字符串中，出现在一个字符数组中任意字符最后一次出现的位置
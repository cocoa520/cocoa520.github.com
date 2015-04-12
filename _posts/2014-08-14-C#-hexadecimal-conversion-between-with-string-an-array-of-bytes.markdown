---
layout: post
title: "C#16进制与字符串、字节数组之间的转换"
date: 2014-08-14 22:40:00 
comments: true
categories: [C#]
tags: [C#]
description: 整理一些常用的C#16进制与字符串、字节数组之间的转换的函数，以方便需要之时使用。
keywords: C# byte string

---

整理一些常用的进制与字符串、字节数组之间的转换的函数，以方便需要之时使用。

{% highlight C# %}

/// <summary>; 
/// 16进制字符串转为16进制字符数组 
/// </summary>; 
/// <param name="newString">;</param>; 
/// <returns>;</returns>; 
public static byte[] Hex2ByteArr(string newString)
{
    int len = newString.Length / 2;
    byte[] arr = new byte[len];
    for (int i = 0; i < len; i++)
    {
        arr[i] = Convert.ToByte(newString.Substring(i * 2, 2), 16);
    }
    return arr;
}

/// <summary>; 
/// 字符串转16进制字节数组 
/// </summary>; 
/// <param name="hexString">;</param>; 
/// <returns>;</returns>; 
private static byte[] strToToHexByte(string hexString) 
{ 
	hexString = hexString.Replace(" ", ""); 
	if ((hexString.Length % 2) != 0) 
		hexString += " "; 
	byte[] returnBytes = new byte[hexString.Length / 2]; 
	for (int i = 0; i < returnBytes.Length; i++) 
		returnBytes[i] = Convert.ToByte(hexString.Substring(i * 2, 2), 16); 
	return returnBytes; 
} 

/// <summary>; 
/// 字节数组转16进制字符串 
/// </summary>; 
/// <param name="bytes">;</param>; 
/// <returns>;</returns>; 
public static string byteToHexStr(byte[] bytes) 
{ 
	string returnStr = ""; 
	if (bytes != null) 
	{ 
		for (int i = 0; i < bytes.Length; i++) 
		{ 
			returnStr += bytes[i].ToString("X2"); 
		} 
	} 
	return returnStr; 
} 

/// <summary>; 
/// 从汉字转换到16进制 
/// </summary>; 
/// <param name="s">;</param>; 
/// <param name="charset">;编码,如"utf-8","gb2312"</param>; 
/// <param name="fenge">;是否每字符用逗号分隔</param>; 
/// <returns>;</returns>; 
public static string ToHex(string s, string charset, bool fenge) 
{ 
	if ((s.Length % 2) != 0) 
	{ 
		s += " ";//空格 
		//throw new ArgumentException("s is not valid chinese string!"); 
	} 
	System.Text.Encoding chs = System.Text.Encoding.GetEncoding(charset); 
	byte[] bytes = chs.GetBytes(s); 
	string str = ""; 
	for (int i = 0; i < bytes.Length; i++) 
	{ 
		str += string.Format("{0:X}", bytes[i]); 
		if (fenge && (i != bytes.Length - 1)) 
		{ 
			str += string.Format("{0}", ","); 
		} 
	} 
	return str.ToLower(); 
} 

///<summary>; 
/// 从16进制转换成汉字 
/// </summary>; 
/// <param name="hex">;</param>; 
/// <param name="charset">;编码,如"utf-8","gb2312"</param>; 
/// <returns>;</returns>; 
public static string UnHex(string hex, string charset) 
{ 
	if (hex == null) 
	throw new ArgumentNullException("hex"); 
	hex = hex.Replace(",", ""); 
	hex = hex.Replace("\n", ""); 
	hex = hex.Replace("\\", ""); 
	hex = hex.Replace(" ", ""); 
	if (hex.Length % 2 != 0)
	{ 
		hex += "20";//空格 
	} 
	// 需要将 hex 转换成 byte 数组。 
	byte[] bytes = new byte[hex.Length / 2]; 
	for (int i = 0; i < bytes.Length; i++) 
	{ 
		try 
		{ 
			// 每两个字符是一个 byte。 
			bytes[i] = byte.Parse(hex.Substring(i * 2, 2), System.Globalization.NumberStyles.HexNumber); 
		} 
		catch 
		{ 
			// Rethrow an exception with custom message. 
			throw new ArgumentException("hex is not a valid hex number!", "hex"); 
		} 
	} 
	System.Text.Encoding chs = System.Text.Encoding.GetEncoding(charset); 
	return chs.GetString(bytes); 
}

{% endhighlight %} 
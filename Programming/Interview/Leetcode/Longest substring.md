https://leetcode.com/problems/longest-substring-without-repeating-characters/

дана строка нужно в ней найти максимально длинную подстроку.
цикл по символам
2 указателя(start, end) указывающие на начало строки
Используем словарь для хранения символов
как только доходим до символа который есть в коллекции strart++ и end = start, сохраняем длину подстроки и очищаем словарь

```cs
int LengthOfLongestSubstring(string s)
{
    var simbols = new Dictionary<char, int>();

    int maxLengthSubstring = 0;
    int start = 0;    
    for (int end = 0; end < s.Length; end++)
    {        
        if (simbols.ContainsKey(s[end]))
        {
            start++;
            end = start;
            simbols.Clear();            
        }
        
        simbols.Add(s[end], end);

        maxLengthSubstring = Math.Max(maxLengthSubstring, end - start + 1);
        var substring = s.Substring(start, end - start+1);
    }

    return maxLengthSubstring;
}
```
# php-ansi-to-html
A simple PHP function that converts ANSI color codes to HTML

```php
function ansi_to_html($text) {
    $ansi_codes = [
        '0;30' => '<span style="color: black;">',
        '0;31' => '<span style="color: red;">',
        '0;32' => '<span style="color: green;">',
        '0;33' => '<span style="color: yellow;">',
        '0;34' => '<span style="color: blue;">',
        '0;35' => '<span style="color: purple;">',
        '0;36' => '<span style="color: cyan;">',
        '0;37' => '<span style="color: white;">',
        '1;30' => '<span style="color: black; font-weight: bold;">',
        '1;31' => '<span style="color: red; font-weight: bold;">',
        '1;32' => '<span style="color: green; font-weight: bold;">',
        '1;33' => '<span style="color: yellow; font-weight: bold;">',
        '1;34' => '<span style="color: blue; font-weight: bold;">',
        '1;35' => '<span style="color: purple; font-weight: bold;">',
        '1;36' => '<span style="color: cyan; font-weight: bold;">',
        '1;37' => '<span style="color: white; font-weight: bold;">',
        '0' => '</span>'
    ];

    $regex = '/\033\[([0-9;]+)m/';

    return preg_replace_callback($regex, function($matches) use ($ansi_codes) {
        if (isset($ansi_codes[$matches[1]])) {
            return $ansi_codes[$matches[1]];
        }
        return '';
    }, $text);
}
```

This function does the following:

1. We define an array `$ansi_codes` that maps ANSI color codes to their corresponding HTML span tags with inline CSS.

2. We use a regular expression to match ANSI escape sequences in the text.

3. We use `preg_replace_callback` to replace each ANSI escape sequence with its corresponding HTML span tag.

4. If a matching HTML span tag is found for the ANSI code, it's returned. Otherwise, an empty string is returned (effectively removing unsupported ANSI codes).

5. The function returns the modified text with ANSI codes replaced by HTML span tags.

You can use this function like this:

```php
$ansi_text = "\033[0;31mThis is red text\033[0m and \033[1;34mthis is bold blue text\033[0m";
$html_text = ansi_to_html($ansi_text);
echo $html_text;
```

This will output:

```html
<span style="color: red;">This is red text</span> and <span style="color: blue; font-weight: bold;">this is bold blue text</span>
```

Note that this is a basic implementation and doesn't cover all possible ANSI codes. It handles the most common color and bold codes. You may need to expand it if you want to support more ANSI features like background colors, underline, etc.

Also, remember that the output of this function is HTML, so you should use it in an HTML context. If you're outputting to a webpage, make sure you're sending the appropriate content type header:

```php
header('Content-Type: text/html; charset=utf-8');
```

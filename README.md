# PHP lexical analyzer

PHP implementation of Lexical Analyzer.

> **Warning**
> This is not a GENERATOR like classical lex is. It does not produce any php code. It's a simple plain scanner
> of the given input string and tokenizer into given set of tokens by matching regular expressions.
> Thus, at runtime you can change the token definition and use one same code for any token set.

# Token Definition

Tokens are defined with ``TokenDefinition`` class that holds token name and regular expression. Token name can be
empty, and in that case, lexer will ignore/skip such tokens.

# Lexer Configuration

The lexer configuration holds a list of all token definitions. With ``LexerArrayConfig`` it can be easily created from an array
where keys are regular expressions and values are names of tokens.

# Full scan

Lexer's static method ``scan($config, $input)`` can be used to scan given input string and return an array of tokens.

# Lexer with state

Instance of the ``Lexer`` class be used to walk trough scanned tokens with single look-ahead token.

It's similar in API to [doctrine/lexer](https://github.com/doctrine/lexer), just tokens are defined and scanned differently, w/out
the need for recognizing the token type/name from the tokenized value - rather the token type/name is given by the same ``TokenDefn``
that gave the regex to recognize the token.


# Examples

``` php
<?php
$config = new LexerArrayConfig([
            '\\s' => '',
            '\\d+' => 'number',
            '\\+' => 'plus',
            '-' => 'minus',
            '\\*' => 'mul',
            '/' => 'div',
        ]);

// static scan method that returns an array of
$tokens = Lexer::scan($config, '2 + 3');
array_map(function ($t) { return $this->getName(); }, $tokens); // ['number', 'plus', 'number']

// lexer instance
$lexer = new Lexer($config);
$lexer->setInput('2 + 3');
$lexer->moveNext();
while ($lexer->getLookahead()) {
    print $lexer->getLookahead()->getName();
    $lexer->moveNext();
}
```

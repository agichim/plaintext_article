**This repo is a fork with small changes.**

- saves article title as `article.thisisasavedarticle.md`
- remove extra characters from title:
  - simple quotation mark `'`
  - hyphen `-`
- don't print to screen when saving article as a file

--
# Plaintext Article

A simple python script I use for reading articles in plain text.

It depends on [txtify.it](https://txtify.it), and is intended for moderate use (for too many requests you should use their API).

## Usage

Print article on screen

```
$ python plaintext_article.py https://articles.xyz/some-article
```

Save it as an `.md` file.

```
$ python plaintext_article.py https://articles.xyz/some-article -s
```


Running something like:
```bash
$ echo "https://i.redd.it/4pfwbtqsaby41.jpg" | xargs wget -O meme.jpg -q
```
would be equavalent to running:
```bash
$ wget -O meme.jpg -q "https://i.redd.it/4pfwbtqsaby41.jpg"
```
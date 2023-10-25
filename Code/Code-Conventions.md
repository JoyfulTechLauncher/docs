# Flutter Code Conventions

## Basic rules
Please refer to this [link](https://github.com/flutter/flutter/wiki/Style-guide-for-Flutter-repo)

## In Our Repo
Code of good quality should explain itself. For others' convenience, you should make sure that you use readable method names, variable names and parameter names. E.g.:

1.
```
**Don't use ambigious naming:**

int ab = 100
```

2.
```
**Don't use abbreviations:**

private Future<String> getSN(String APIPath) {
	...
}

**use:**

private Future<String> getServerName(String APIPath) {
	...
}
```

3. Make sure you comment on what the code does in a high level.

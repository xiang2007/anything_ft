# Libft: function checklist (summary)

Personal summary of the required functions. Not a copy of the subject.

## Rules
- Language: C, Norm-compliant. Compile every `.c` with `-Wall -Wextra -Werror` using `cc`.
- No global variables. Helper functions must be `static`.
- No crashes (segfault, double free, ...) and no memory leaks.
- All files at the repo root. No unused files.
- Output: `libft.a`, built with `ar` (NOT `libtool`), at the repo root.
- Submit: `Makefile`, `libft.h`, `ft_*.c`.
- Makefile rules: `$(NAME)`, `all`, `clean`, `fclean`, `re` (no needless relinking).
- Do not use `restrict` in prototypes and do not compile with `-std=c99`.
- `README.md` at the root:
  - First line in italics: *This project has been created as part of the 42 curriculum by <login>.*
  - Sections: Description, Instructions, Resources (including how AI was used), plus a detailed description of the library.

## Language implementations
This project is written in multiple languages. Each language gets its own directory at the repo root, named `<language name>libft`, with the language name in camel case (first letter lowercase).

| Language | Directory |
|---|---|
| C | `cLibft` |

Rules:
- Each `<language name>libft` directory is a self-contained implementation of the same function list below.
- Add a new language by creating `<language name>libft` alongside the existing ones. Do not put other languages' files at the repo root.

## Part 1: libc re-implementations
Same prototype and behaviour as the man page, with an `ft_` prefix. No external functions allowed, except `malloc` for `calloc` and `strdup`.

| Group | Functions |
|---|---|
| Char checks (return 1 or 0) | `isalpha` `isdigit` `isalnum` `isascii` `isprint` |
| Case | `toupper` `tolower` |
| Strings | `strlen` `strlcpy` `strlcat` `strchr` `strrchr` `strncmp` `strnstr` `atoi` |
| Memory | `memset` `bzero` `memcpy` `memmove` `memchr` `memcmp` |
| Uses `malloc` | `calloc` `strdup` |

Notes:
- `calloc`: if `nmemb` or `size` is 0, return a unique pointer that can be passed to `free()`.
- `strlcpy`, `strlcat` and `bzero` are BSD functions. To test against the system versions on glibc, include `<bsd/string.h>` and link with `-lbsd`.

## Part 2: additional functions
Allowed externals are noted in brackets.

| Function | Prototype | What it does |
|---|---|---|
| `ft_substr` | `char *(char const *s, unsigned int start, size_t len)` | New string from `s`, starting at `start`, at most `len` chars. NULL on alloc fail. [malloc] |
| `ft_strjoin` | `char *(char const *s1, char const *s2)` | New string, `s1` followed by `s2`. [malloc] |
| `ft_strtrim` | `char *(char const *s1, char const *set)` | Copy of `s1` with chars in `set` removed from both ends. [malloc] |
| `ft_split` | `char **(char const *s, char c)` | NULL-terminated array of strings split on `c`. Each string and the array are malloc'd. NULL if any alloc fails (free what was already allocated). [malloc, free] |
| `ft_itoa` | `char *(int n)` | Integer to string. Handles negatives. [malloc] |
| `ft_strmapi` | `char *(char const *s, char (*f)(unsigned int, char))` | New string made by applying `f(index, char)` to each char. [malloc] |
| `ft_striteri` | `void (char *s, void (*f)(unsigned int, char *))` | Applies `f(index, &char)` to each char in place. |
| `ft_putchar_fd` | `void (char c, int fd)` | Write a char to `fd`. [write] |
| `ft_putstr_fd` | `void (char *s, int fd)` | Write a string to `fd`. [write] |
| `ft_putendl_fd` | `void (char *s, int fd)` | Write a string then `\n` to `fd`. [write] |
| `ft_putnbr_fd` | `void (int n, int fd)` | Write an int to `fd`. [write] |

## Part 3: linked list
Add this to `libft.h`:

```c
typedef struct s_list
{
	void			*content;
	struct s_list	*next;
}	t_list;
```

| Function | Prototype | What it does |
|---|---|---|
| `ft_lstnew` | `t_list *(void *content)` | Allocate a node with `content`, `next = NULL`. [malloc] |
| `ft_lstadd_front` | `void (t_list **lst, t_list *new)` | Insert `new` at the head. |
| `ft_lstsize` | `unsigned int (t_list *lst)` | Count the nodes. |
| `ft_lstlast` | `t_list *(t_list *lst)` | Return the last node. |
| `ft_lstadd_back` | `void (t_list **lst, t_list *new)` | Append `new` at the tail. |
| `ft_lstdelone` | `void (t_list *lst, void (*del)(void *))` | Free the content with `del`, then free the node. Does NOT free `next`. [free] |
| `ft_lstclear` | `void (t_list **lst, void (*del)(void *))` | Delete the node and all after it, then set `*lst = NULL`. [free] |
| `ft_lstiter` | `void (t_list *lst, void (*f)(void *))` | Apply `f` to each node's content. |
| `ft_lstmap` | `t_list *(t_list *lst, void *(*f)(void *), void (*del)(void *))` | Build a new list from `f(content)`. On alloc failure, clear the partial list with `del`. [malloc, free] |

## Progress
- [ ] Part 1
- [ ] Part 2
- [ ] Part 3
- [ ] Makefile
- [ ] README.md

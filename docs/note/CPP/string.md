---
    slug: string
    title: C++ string 相关
    date: 2024-04-01
    authors: Kaesar
    tags: [C++, string]
    keywords: [C++, STL, string]
---

#### 1. 实现 `split` 功能

- 方法1：`stringstream` 和 `getline` 配合使用
```c++
std::vector<std::string> stringSplit(const std::string& str, char delim) {
    std::stringstream ss(str);
    std::string item;
    std::vector<std::string> elems;

    while (std::getline(ss, item, delim)) {
        if (!item.empty()) {
            elems.push_back(item);
        }
    }

    return elems;
}
```

- 方法2：使用 `std::string::find`
```c++
std::vector<std::string> stringSplit(const std::string& str, char delim) {
    std::size_t previous = 0;
    std::size_t current = str.find(delim);
    std::vector<std::string> elems;

    while (current != std::string::npos) {
        if (current > previous) {
            elems.push_back(str.substr(previous, current - previous));
        }
        previous = current + 1;
        current = str.find(delim, previous);
    }

    if (previous != str.size()) {
        elems.push_back(str.substr(previous));
    }

    return elems;
}
```
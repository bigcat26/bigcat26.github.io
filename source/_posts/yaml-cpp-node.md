---
title: yaml-cpp按路径获取节点的方法
date: 2022-05-28
tags: 
  - yaml
  - c++
---

代码先丢在这，文字以后再补

```c++
static inline std::vector<std::string> stringSplit(const std::string &str,
                                                   const char delimiter) {
  std::string tok;
  std::stringstream ss(str);
  std::vector<std::string> vec;
  while (std::getline(ss, tok, delimiter)) {
    vec.push_back(tok);
  }
  return vec;
}

static inline YAML::Node getNodeByPath(YAML::Node root,
                                       const std::string &path) {
  std::vector<YAML::Node> n{root};
  auto parts = stringSplit(path, '.');
  for (const auto &name : parts) {
    // std::string lower;
    // std::transform(part.begin(), part.end(), std::back_inserter(lower),
    //                tolower);
    if (auto child = n[n.size() - 1][name]) {
      n.push_back(child);
    } else {
      return YAML::Node();
    }
  }
  return n[n.size() - 1];
}

template <typename... Args>
static inline YAML::Node getNode(YAML::Node root, Args... args) {
  std::vector<YAML::Node> n{root};
  for (const auto &name : {args...}) {
    if (auto child = n[n.size() - 1][name]) {
      n.push_back(child);
    } else {
      return YAML::Node();
    }
  }
  return n[n.size() - 1];
}

TEST_CASE("parse", "[yaml]") {

  auto cfg = YAML::LoadFile("/home/chris/work/cxx/full.yml");

  auto nodeA = getNodeByPath(cfg, "root.certificate.ca");
  auto nodeB = getNodeByPath(cfg, "root.certificate.client");
  auto nodeC = getNode(cfg, "root", "cloud", "server");
  auto nodeD = getNode(cfg, "root", "database", "path");

  SPDLOG_INFO("cfg = {}", YAML::Dump(cfg));
  SPDLOG_INFO("nodeA = {}", YAML::Dump(nodeA));
  SPDLOG_INFO("nodeB = {}", YAML::Dump(nodeB));
  SPDLOG_INFO("nodeC = {}", YAML::Dump(nodeC));
  SPDLOG_INFO("nodeD = {}", YAML::Dump(nodeD));
}
```

TO BE CONTINUE...
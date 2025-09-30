# Yizhong Liu's Academic Website

Deployed on Github Pages: [https://yizhongliu1991.github.io/](https://yizhongliu1991.github.io/)

Run Local：bundle binstubs jekyll

## macOS 本地运行与问题排查

以下步骤可在 Apple Silicon 上稳定运行，并避免系统 Ruby 权限/编译问题：

1) 安装依赖（需 Homebrew 与 Xcode Command Line Tools）

```bash
brew install ruby node
# 若未安装命令行工具，可运行（如已安装会提示）：
# xcode-select --install
```

2) 配置 Ruby 到 PATH（新开终端需再次生效，建议写入 shell 配置）

```bash
export PATH="/opt/homebrew/opt/ruby/bin:$PATH"
echo 'export PATH="/opt/homebrew/opt/ruby/bin:$PATH"' >> ~/.zshrc
```

3) 安装 Bundler（用户态）

```bash
gem install bundler --no-document
```

4) 将依赖安装到本地目录，避免系统目录权限问题

```bash
bundle config set --local path 'vendor/bundle'
```

5) 若安装依赖时遇到 `eventmachine`/C++ 头文件相关错误（如 `iostream not found`），显式指定 macOS SDK 与编译标志后再安装

```bash
export SDKROOT="$(xcrun --show-sdk-path)"
bundle config set build.eventmachine "--with-cppflags=-I$SDKROOT/usr/include/c++/v1 -I$SDKROOT/usr/include --with-cxxflags=-std=c++17 --with-ldflags=-L$SDKROOT/usr/lib"
```

6) 安装项目依赖

```bash
bundle install
```

7) 启动本地预览（推荐始终通过项目锁定的依赖运行）

```bash
bundle exec jekyll serve -l -H localhost
# 浏览器打开 http://localhost:4000
```

8) 可选：使用 binstubs 方便直接运行（仍然使用项目内依赖）

```bash
bundle binstubs jekyll
./bin/jekyll serve -l -H localhost
```

说明：
- 推荐使用 `bundle exec jekyll ...`，以确保使用 `Gemfile`/`Gemfile.lock` 中锁定的版本，避免全局 gem 版本不匹配。
- 如重开终端未生效，请先执行 `export PATH="/opt/homebrew/opt/ruby/bin:$PATH"` 或重新加载 `~/.zshrc`。
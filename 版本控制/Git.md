# Git

## 上传大文件

如果您需要上传大文件到 Git 仓库，可以使用 Git LFS（Large File Storage）。

Git LFS 是一个开源的 Git 扩展，它允许您将大型二进制文件存储在独立于 Git 仓库的存储库中。这意味着您可以在不影响 Git 仓库大小和性能的情况下轻松地存储和分享大型文件。

要使用 Git LFS，请执行以下步骤：
1. 安装 Git LFS：在命令行运行`git lfs install`命令以安装 Git LFS。
2. 跟踪大文件：在您的 Git 仓库中，运行`git lfs track "<pattern>"`命令来跟踪大文件。 例如，如果您要跟踪所有名为 "*.jpg" 的 JPG 图像文件，则可以运行`git lfs track "*.jpg"`命令。
3. 添加和提交大文件：添加和提交大文件与普通文件一样，使用`git add <file>`和`git commit -m "<message>"`命令即可。

请注意，使用 Git LFS 可能会导致一些额外的费用，因为 Git LFS 存储库是由第三方提供商托管的。如果您使用 GitHub 或 GitLab 等 Git 托管服务，可能需要购买额外的存储空间或付费扩展。

推送到远端仓库：使用 git push origin <branch> 命令将提交的更改推送到远程 Git 仓库。

## 特殊文件
- `.gitkeep文件`：由于Git无法追踪一个空的文件夹，当用户需要追踪(track)一个空的文件夹的时候，按照惯例，大家会把一个称为.gitkeep的文件放在这些文件夹里。

- `.gitignore文件`：告诉Git不需要追踪指定的某些文件，详见[官网](https://git-scm.com/docs/gitignore)

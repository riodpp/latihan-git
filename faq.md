# Day 12 FAQ: Git dan GitHub

## 1. Apa bedanya Git dan GitHub?

Git adalah tool version control untuk melacak perubahan code. GitHub adalah platform hosting repository Git di web.

```bash
git status
git push origin main
```

## 2. Apa itu repository?

Repository adalah storage project yang melacak file dan perubahan. Setelah `git init`, Git membuat folder `.git`.

```bash
git init
```

## 3. Apa bedanya working directory, staging area, dan commit?

Working directory adalah file yang sedang diedit. Staging area adalah perubahan yang siap dikomit. Commit adalah snapshot project.

```bash
git add .
git commit -m "feat: add pipeline"
```

## 4. Apa bedanya tracked dan untracked?

Tracked berarti Git sudah mengenal file. Untracked berarti file baru belum pernah ditambahkan ke Git.

```bash
git status
git add pipeline.py
```

## 5. Kapan pakai `git diff`?

Pakai `git diff` sebelum commit untuk memastikan perubahan memang sesuai.

```bash
git diff
git diff --cached
```

## 6. Apa itu commit message yang baik?

Commit message harus jelas dan menjelaskan intent perubahan.

```bash
git commit -m "feat(validation): add order status validation"
git commit -m "fix(order): correct subtotal calculation"
```

## 7. Apa bedanya checkout, revert, dan reset?

`checkout` melihat state commit/branch lain. `revert` membatalkan commit dengan commit baru. `reset` memindahkan branch pointer dan bisa menghapus commit.

```bash
git checkout <commit_id>
git revert <commit_id>
git reset <commit_id>
```

Hati-hati dengan:

```bash
git reset <commit_id> --hard
```

Karena perubahan setelah target bisa hilang.

## 8. Apa fungsi branch?

Branch adalah ruang kerja terpisah untuk fitur/fix tanpa mengganggu main branch.

```bash
git checkout -b feature/order-validation
```

## 9. Apa itu merge conflict?

Conflict terjadi ketika Git tidak bisa otomatis menggabungkan perubahan, misalnya dua orang mengubah baris/file yang sama.

```bash
git status
git add file_yang_sudah_diresolve
git commit -m "fix: resolve merge conflict"
```

## 10. Apa bedanya merge biasa dan squash merge?

Merge biasa membawa commit history branch feature. Squash merge hanya membawa perubahan sebagai satu commit baru.

```bash
git merge feature/order-validation
git merge --squash feature/order-validation
```

## 11. Apa fungsi stash?

Stash menyimpan perubahan sementara saat belum siap commit.

```bash
git stash
git stash pop
```

## 12. Apa golden rule rebase?

Jangan rebase public branch. Rebase menulis ulang history, sehingga bisa membingungkan teammate yang sudah pull branch tersebut.

```bash
git rebase main
```

## 13. Apa fungsi pull request?

Pull request adalah permintaan untuk menggabungkan perubahan dari satu branch ke branch lain. PR review membantu menemukan bug, risiko, dan memastikan standar code.

## 14. Branching strategy apa yang umum?

GitHub Flow sederhana: satu `main` production-ready dan feature branch pendek. GitFlow lebih kompleks dengan feature/release/hotfix. GitLab Flow menggabungkan feature branch dengan issue tracking dan environment seperti staging.

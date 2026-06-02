# Day 12 Exercise: Git dan GitHub

> Materi: version control, Git repository, GitHub, konfigurasi Git, basic Git command, commit lifecycle, commit message, checkout, revert, reset, branch, merge, merge conflict, stash, rebase, branching strategy, pull request, dan PR review.

Latihan dibuat sebagai use case Data Engineer: mengelola mini project pipeline order buah.

## A. Git Configuration dan Repository

### 1. Cek dan set konfigurasi Git

Tulis command untuk:

- cek versi Git
- cek username Git
- cek email Git
- set global username
- set global email
- membuka Git help

### 2. Membuat repository baru

Buat folder project bernama `fruit_orders_pipeline`, lalu inisialisasi sebagai Git repository.

### 3. Git vs GitHub

Jelaskan perbedaan Git dan GitHub berdasarkan slide.

### 4. Remote repository

Tulis command untuk menambahkan remote origin dan push branch `main` ke remote.

Gunakan format:

```bash
git remote add origin <remote_url>
git push -u origin main
```

### 5. .gitignore

Buat `.gitignore` untuk project Python/Data Engineer yang mengabaikan:

- `__pycache__/`
- `*.pyc`
- `.env`
- `playground_output/`
- `tmp/`

## B. Status Lifecycle dan Commit

### 6. Tracked dan Untracked

Jelaskan perbedaan tracked dan untracked files.

### 7. Add, status, diff, commit

Buat file `pipeline.py`, lalu tulis command untuk:

- melihat status
- melihat diff
- stage semua file
- commit dengan message yang baik

### 8. Commit history

Tulis command untuk melihat commit history secara ringkas.

### 9. Perfect commit

Jelaskan dua prinsip perfect commit dari slide:

- add the right changes
- compose a good commit message

### 10. Conventional commit

Buat contoh commit message untuk:

- fitur baru data validation
- bug fix subtotal calculation
- dokumentasi README
- update `.gitignore`

## C. Checkout, Revert, Reset

### 11. Checkout commit

Jelaskan fungsi `git checkout <commit_id>` untuk melihat state commit lama, lalu command untuk kembali ke branch `main`.

### 12. Revert

Jelaskan fungsi `git revert <commit_id>`.

### 13. Reset

Jelaskan perbedaan:

```bash
git reset <commit_id>
git reset <commit_id> --hard
```

## D. Branch, Merge, dan Conflict

### 14. Branch basic

Tulis command untuk:

- membuat branch `feature/data-quality-report`
- melihat semua branch
- pindah ke branch tersebut
- membuat dan langsung pindah ke branch `feature/order-validation`
- menghapus branch

### 15. Merge

Tulis command untuk merge branch `feature/order-validation` ke `main`.

Berikan dua versi:

- merge biasa
- merge squash

### 16. Merge conflict

Sebutkan tiga penyebab conflict dari slide.

### 17. Resolve conflict

Buat langkah umum menyelesaikan conflict:

- cek status
- buka file conflict
- pilih/gabungkan isi yang benar
- add file
- commit hasil resolve

## E. Stash dan Rebase

### 18. Stash

Tulis command untuk menyimpan perubahan sementara dan mengembalikannya.

### 19. Rebase

Jelaskan apa yang dilakukan `git rebase main` pada feature branch.

### 20. Golden rule rebase

Apa golden rule dari slide tentang rebase?

## F. Branching Strategy dan Pull Request

### 21. Branching strategy

Jelaskan ringkas:

- GitHub Flow
- GitFlow
- GitLab Flow

### 22. Pull Request

Jelaskan fungsi pull request dan PR review.

### 23. Challenge: Mini Git Workflow DE

Buat urutan command untuk workflow ini:

1. init repo `fruit_orders_pipeline`
2. buat `.gitignore`
3. buat `pipeline.py`
4. commit awal
5. buat branch `feature/order-validation`
6. tambah file `validation.py`
7. commit dengan conventional commit
8. merge branch ke `main`
9. lihat commit history

### 24. Challenge: Simulasi conflict

Buat skenario singkat untuk menghasilkan merge conflict pada file `reports/summary.txt`, lalu tulis command umum untuk menyelesaikannya.

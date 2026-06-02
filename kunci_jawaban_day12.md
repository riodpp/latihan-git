# Kunci Jawaban Day 12: Git dan GitHub

> Fokus kunci jawaban ini adalah command Git yang perlu dijalankan dan penjelasan konsep. Tidak perlu menjalankan script Python.

## A. Git Configuration dan Repository

### 1. Cek dan set konfigurasi Git

Command:

```bash
git --version
git config user.name
git config user.email
git config --global user.name "Nama Kamu"
git config --global user.email "email@example.com"
git --help
```

Penjelasan:

- `git --version` mengecek versi Git yang terinstall.
- `git config user.name` dan `git config user.email` mengecek konfigurasi nama dan email pada repository aktif.
- `git config --global ...` menyimpan konfigurasi default untuk semua repository di komputer.
- `git --help` membuka bantuan umum Git.

### 2. Membuat repository baru

Command:

```bash
mkdir fruit_orders_pipeline
cd fruit_orders_pipeline
git init
```

Penjelasan:

`mkdir` membuat folder project, `cd` masuk ke folder tersebut, lalu `git init` membuat repository Git lokal. Setelah command ini, Git membuat folder tersembunyi bernama `.git`.

### 3. Git vs GitHub

Jawaban:

Git adalah tool version control untuk melacak perubahan file dan history commit secara lokal. GitHub adalah platform web untuk menyimpan repository Git secara remote dan mendukung kolaborasi seperti pull request, review, issue, dan project management.

### 4. Remote repository

Command:

```bash
git remote add origin <remote_url>
git push -u origin main
```

Penjelasan:

`git remote add origin` menghubungkan repository lokal ke repository remote. `git push -u origin main` mengirim branch `main` ke remote dan menyimpan tracking branch agar push berikutnya bisa cukup memakai `git push`.

### 5. `.gitignore`

Command:

```bash
cat > .gitignore <<'EOF'
__pycache__/
*.pyc
.env
playground_output/
tmp/
EOF
```

Isi `.gitignore`:

```gitignore
__pycache__/
*.pyc
.env
playground_output/
tmp/
```

Penjelasan:

`.gitignore` digunakan agar file sementara, cache, environment secret, dan folder output lokal tidak ikut masuk commit.

## B. Status Lifecycle dan Commit

### 6. Tracked dan Untracked

Jawaban:

Tracked file adalah file yang sudah dikenal Git, biasanya karena pernah di-commit atau sudah masuk staging area. Untracked file adalah file baru yang belum dikenal Git dan belum pernah di-add.

Command untuk mengecek:

```bash
git status
```

### 7. Add, status, diff, commit

Command:

```bash
touch pipeline.py
git status
git diff
git add .
git commit -m "feat(pipeline): add order pipeline script"
```

Penjelasan:

- `touch pipeline.py` membuat file kosong.
- `git status` melihat kondisi working directory dan staging area.
- `git diff` melihat perubahan yang belum di-stage.
- `git add .` memasukkan semua perubahan ke staging area.
- `git commit -m ...` menyimpan snapshot perubahan dengan message yang jelas.

### 8. Commit history

Command:

```bash
git log --oneline
```

Penjelasan:

`git log --oneline` menampilkan history commit dalam format ringkas: commit ID pendek dan commit message.

### 9. Perfect commit

Jawaban:

Prinsip pertama adalah `add the right changes`: satu commit sebaiknya berisi perubahan yang masih satu konteks, misalnya hanya fitur validasi atau hanya bug fix subtotal. Prinsip kedua adalah `compose a good commit message`: commit message harus menjelaskan maksud perubahan, bukan sekadar menulis "update" atau "fix".

### 10. Conventional commit

Contoh commit message:

```text
feat(validation): add data validation
fix(order): correct subtotal calculation
docs: update README
chore: update gitignore rules
```

Penjelasan:

`feat` untuk fitur baru, `fix` untuk perbaikan bug, `docs` untuk dokumentasi, dan `chore` untuk perubahan pendukung yang bukan fitur utama.

## C. Checkout, Revert, Reset

### 11. Checkout commit

Command:

```bash
git checkout <commit_id>
git checkout main
```

Penjelasan:

`git checkout <commit_id>` digunakan untuk melihat kondisi project pada commit lama. Saat checkout commit ID secara langsung, posisi Git biasanya masuk ke `detached HEAD`. Untuk kembali bekerja di branch utama, jalankan `git checkout main`.

### 12. Revert

Command:

```bash
git revert <commit_id>
```

Penjelasan:

`git revert` membatalkan perubahan dari commit tertentu dengan membuat commit baru. History lama tetap aman dan tidak dihapus, sehingga cocok untuk branch yang sudah dibagikan ke remote atau dipakai tim.

### 13. Reset

Command:

```bash
git reset <commit_id>
git reset <commit_id> --hard
```

Penjelasan:

`git reset <commit_id>` memindahkan posisi branch ke commit target, tetapi perubahan setelah commit tersebut masih tersisa di working directory. `git reset <commit_id> --hard` memindahkan branch ke commit target sekaligus menghapus perubahan setelah commit tersebut dari working directory. `--hard` harus dipakai hati-hati karena perubahan bisa hilang.

## D. Branch, Merge, dan Conflict

### 14. Branch basic

Command:

```bash
git branch feature/data-quality-report
git branch -a
git checkout feature/data-quality-report
git checkout -b feature/order-validation
git checkout main
git branch -D feature/data-quality-report
```

Penjelasan:

- `git branch <nama_branch>` membuat branch baru.
- `git branch -a` melihat semua branch lokal dan remote.
- `git checkout <nama_branch>` pindah ke branch tertentu.
- `git checkout -b <nama_branch>` membuat branch baru sekaligus pindah ke branch tersebut.
- `git branch -D <nama_branch>` menghapus branch lokal secara paksa.

### 15. Merge

Merge biasa:

```bash
git checkout main
git merge feature/order-validation
```

Merge squash:

```bash
git checkout main
git merge --squash feature/order-validation
git commit -m "feat(validation): add order validation"
```

Penjelasan:

Merge biasa menggabungkan branch feature ke `main` dengan membawa history commit dari branch tersebut. Squash merge menggabungkan seluruh perubahan dari branch feature menjadi satu commit baru di `main`.

### 16. Merge conflict

Tiga penyebab conflict:

- Dua orang mengubah baris yang sama pada file yang sama.
- Satu branch menghapus file, sementara branch lain mengubah file tersebut.
- Dua branch membuat perubahan berbeda pada bagian file yang tidak bisa digabung otomatis oleh Git.

### 17. Resolve conflict

Command umum:

```bash
git status
nano <file_conflict>
git add <file_conflict>
git commit -m "fix: resolve merge conflict"
```

Penjelasan:

Pertama, gunakan `git status` untuk melihat file yang conflict. Buka file tersebut, lalu hapus marker conflict seperti `<<<<<<<`, `=======`, dan `>>>>>>>` setelah memilih atau menggabungkan isi yang benar. Setelah file rapi, jalankan `git add`, lalu commit hasil penyelesaian conflict.

## E. Stash dan Rebase

### 18. Stash

Command:

```bash
git stash
git stash list
git stash pop
```

Penjelasan:

`git stash` menyimpan perubahan sementara tanpa commit. `git stash list` melihat daftar stash. `git stash pop` mengembalikan perubahan terakhir dari stash ke working directory.

### 19. Rebase

Command:

```bash
git checkout feature/order-validation
git rebase main
```

Penjelasan:

`git rebase main` pada feature branch memindahkan commit feature agar seolah-olah dibuat dari ujung terbaru branch `main`. Ini membuat history terlihat lebih linear, tetapi rebase membuat ulang commit ID.

### 20. Golden rule rebase

Jawaban:

Golden rule rebase adalah jangan melakukan rebase pada public branch atau branch yang sudah dipakai orang lain. Karena rebase menulis ulang history, teammate yang sudah pull branch tersebut bisa mengalami konflik history.

## F. Branching Strategy dan Pull Request

### 21. Branching strategy

Jawaban:

GitHub Flow memakai satu branch utama, biasanya `main`, dan setiap fitur dibuat dari feature branch pendek. Cocok untuk workflow sederhana dan release cepat.

GitFlow memakai beberapa jenis branch seperti `main`, `develop`, `feature`, `release`, dan `hotfix`. Cocok untuk project dengan release cycle yang lebih formal, tetapi lebih kompleks.

GitLab Flow menggabungkan feature branch dengan issue tracking dan environment branch seperti production atau staging. Cocok saat deployment mengikuti environment tertentu.

### 22. Pull Request

Jawaban:

Pull request adalah permintaan untuk menggabungkan perubahan dari satu branch ke branch lain, biasanya dari feature branch ke `main`. PR review adalah proses pengecekan perubahan oleh teammate sebelum merge untuk menemukan bug, memastikan standar code, dan memberi masukan teknis.

### 23. Challenge: Mini Git Workflow DE

Command:

```bash
mkdir fruit_orders_pipeline
cd fruit_orders_pipeline
git init -b main

cat > .gitignore <<'EOF'
__pycache__/
*.pyc
.env
playground_output/
tmp/
EOF

cat > pipeline.py <<'EOF'
print("Run fruit orders pipeline")
EOF

git status
git add .
git commit -m "feat(pipeline): add starter order pipeline"

git checkout -b feature/order-validation

cat > validation.py <<'EOF'
def is_valid_order(order):
    return bool(order)
EOF

git status
git add validation.py
git commit -m "feat(validation): add order validation"

git checkout main
git merge feature/order-validation
git log --oneline
```

Penjelasan:

Workflow ini membuat repository baru, menyiapkan file yang tidak perlu di-track, membuat script pipeline awal, lalu menyimpan commit pertama. Setelah itu dibuat feature branch untuk validasi order, commit fitur validasi, kembali ke `main`, merge feature branch, dan melihat history commit.

### 24. Challenge: Simulasi conflict

Skenario:

Branch `main` dan branch `feature/report-quality` sama-sama mengubah file `reports/summary.txt` pada baris yang sama. Saat branch feature di-merge ke `main`, Git tidak bisa memilih isi final secara otomatis sehingga terjadi conflict.

Command membuat conflict:

```bash
mkdir fruit_orders_conflict_demo
cd fruit_orders_conflict_demo
git init -b main

mkdir -p reports
cat > reports/summary.txt <<'EOF'
valid_orders=10
status=initial
EOF

git add .
git commit -m "feat(report): add initial summary"

git checkout -b feature/report-quality
cat > reports/summary.txt <<'EOF'
valid_orders=10
status=checked_by_feature
quality_score=80
EOF

git add reports/summary.txt
git commit -m "feat(report): add quality score"

git checkout main
cat > reports/summary.txt <<'EOF'
valid_orders=10
status=checked_by_main
rejected_orders=2
EOF

git add reports/summary.txt
git commit -m "feat(report): add rejected order metric"

git merge feature/report-quality
```

Command menyelesaikan conflict:

```bash
git status
nano reports/summary.txt
git add reports/summary.txt
git commit -m "fix(report): resolve summary merge conflict"
git log --oneline
```

Contoh isi final `reports/summary.txt` setelah conflict diselesaikan:

```text
valid_orders=10
status=checked_by_main_and_feature
rejected_orders=2
quality_score=80
```

Penjelasan:

Setelah `git merge feature/report-quality`, Git akan menandai file conflict. Buka `reports/summary.txt`, gabungkan informasi penting dari kedua branch, hapus marker conflict, lalu `git add` dan `git commit` untuk menyimpan hasil resolve.

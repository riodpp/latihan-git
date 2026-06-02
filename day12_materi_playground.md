# Day 12 Materi Playground: Git dan GitHub

> Playground ini memakai command terminal langsung. Tidak perlu menjalankan file Python.

Gunakan folder kerja ini agar hasil latihan terpisah dari materi utama:

```bash
mkdir -p day12/playground_output/manual_git_playground
cd day12/playground_output/manual_git_playground
```

## 1. Git Config, Init, dan Repository

Tujuan:

- mengecek instalasi Git
- membuat repository baru
- menyiapkan identitas user Git lokal
- membuat commit pertama

Command:

```bash
git --version

mkdir fruit_orders_repo
cd fruit_orders_repo
git init -b main

git config user.name "Purwadhika DE Student"
git config user.email "student@example.com"

mkdir -p data

cat > .gitignore <<'EOF'
__pycache__/
*.pyc
playground_output/
tmp/
.env
EOF

cat > data/orders.csv <<'EOF'
order_id,product,qty,price,status
ORD-001,apel,2,10000,VALID
ORD-002,jeruk,3,15000,VALID
ORD-003,,1,12000,ERROR_PRODUCT_EMPTY
EOF

cat > pipeline.py <<'EOF'
print("Run fruit orders pipeline")
EOF

git status --short
git add .
git commit -m "feat(pipeline): add fruit order starter project"
git log --oneline
```

Penjelasan:

`git init -b main` membuat repository Git baru dengan branch awal `main`. `git config user.name` dan `git config user.email` diset lokal untuk repository latihan ini. Setelah file sample dibuat, `git add .` memasukkan perubahan ke staging area dan `git commit` menyimpan snapshot pertama.

## 2. Status Lifecycle: Untracked, Modified, Staged, Commit

Pastikan posisi terminal masih di:

```bash
pwd
# output yang diharapkan:
# /Users/riodpp/Documents/Freelance/Purwadhika/day12/playground_output/manual_git_playground/fruit_orders_repo
```

Command:

```bash
cat > README.md <<'EOF'
# Fruit Orders Pipeline

Mini project untuk latihan Git.
EOF

cat > pipeline.py <<'EOF'
print("Run fruit orders pipeline")
print("order_rows=3")
EOF

git status --short
git diff
git add .
git status --short
git diff --cached
git commit -m "docs: add project readme"
git log --oneline --decorate -3
```

Penjelasan:

`git status --short` menunjukkan file yang berubah dalam format ringkas. `git diff` melihat perubahan yang belum masuk staging area. Setelah `git add .`, perubahan pindah ke staging area dan bisa dicek dengan `git diff --cached`. Commit menyimpan perubahan tersebut ke history repository.

## 3. Commit History: Checkout, Revert, Reset

Tujuan:

- membuat beberapa commit
- melihat commit lama
- membatalkan commit dengan `revert`
- mencoba `reset --soft`

Command:

```bash
mkdir -p reports

cat > reports/summary.txt <<'EOF'
valid_orders=2
rejected_orders=1
EOF

git add reports/summary.txt
git commit -m "feat(report): add order summary"

cat > reports/summary.txt <<'EOF'
valid_orders=2
rejected_orders=1
total_revenue=45000
EOF

git add reports/summary.txt
git commit -m "feat(report): add revenue metric"

git log --oneline
```

Checkout commit lama:

```bash
git checkout <commit_id_lama>
git status
git checkout main
```

Revert commit terbaru:

```bash
git log --oneline
git revert <commit_id_yang_ingin_dibatalkan>
git log --oneline -3
```

Reset soft:

```bash
git reset --soft HEAD~1
git status --short
git commit -m "revert(report): remove revenue metric"
```

Penjelasan:

`git checkout <commit_id_lama>` dipakai untuk melihat state lama tanpa pindah branch permanen. `git revert` membuat commit baru yang membatalkan commit tertentu, sehingga aman untuk history yang sudah dibagikan. `git reset --soft HEAD~1` melepas commit terakhir tetapi perubahan tetap berada di staging area. Pada contoh ini, perubahan hasil revert di-commit lagi agar repository kembali bersih sebelum lanjut ke bagian berikutnya.

## 4. Branch, Merge, dan Merge Conflict

Tujuan:

- membuat branch feature
- membuat perubahan berbeda pada file yang sama
- menghasilkan merge conflict
- menyelesaikan conflict

Command membuat branch feature:

```bash
git checkout main

cat > reports/summary.txt <<'EOF'
source=main
valid_orders=2
EOF

git add reports/summary.txt
git commit -m "feat(report): add main summary"

git checkout -b feature/data-quality-report

cat > reports/summary.txt <<'EOF'
source=feature
valid_orders=2
quality_score=80
EOF

git add reports/summary.txt
git commit -m "feat(report): add data quality score"
```

Command membuat perubahan berbeda di `main`:

```bash
git checkout main

cat > reports/summary.txt <<'EOF'
source=main
valid_orders=2
rejected_orders=1
EOF

git add reports/summary.txt
git commit -m "feat(report): add rejected order metric"
```

Command merge sampai conflict:

```bash
git merge feature/data-quality-report
git status --short
cat reports/summary.txt
```

Contoh isi final setelah conflict diselesaikan:

```bash
cat > reports/summary.txt <<'EOF'
source=merged
valid_orders=2
rejected_orders=1
quality_score=80
EOF

git add reports/summary.txt
git commit -m "fix(report): resolve summary merge conflict"
git log --oneline --decorate -4
```

Penjelasan:

Conflict terjadi karena `main` dan `feature/data-quality-report` mengubah bagian yang sama pada `reports/summary.txt`. Saat file conflict dibuka, Git memberi marker `<<<<<<<`, `=======`, dan `>>>>>>>`. Hapus marker tersebut, gabungkan isi yang benar, lalu commit hasil resolve.

## 5. Stash, Rebase, dan Remote Repository Lokal

### Stash

Command:

```bash
git checkout main

cat > pipeline.py <<'EOF'
print("Run fruit orders pipeline")
print("order_rows=3")
print("draft debug log")
EOF

git status --short
git stash
git status --short
git stash list
git stash pop

git add pipeline.py
git commit -m "chore(pipeline): keep debug log example"
```

Penjelasan:

`git stash` menyimpan perubahan sementara saat belum siap commit. Setelah stash, working directory kembali bersih. `git stash pop` mengembalikan perubahan terakhir dari stash.

### Rebase

Command:

```bash
git checkout -b feature/add-validation

cat > validation.py <<'EOF'
def is_valid_status(status):
    return status == "VALID"
EOF

git add validation.py
git commit -m "feat(validation): add status validator"

git checkout main

cat > README.md <<'EOF'
# Fruit Orders Pipeline

Git demo untuk Data Engineer.
EOF

git add README.md
git commit -m "docs: add project overview"

git checkout feature/add-validation
git rebase main
git log --oneline --decorate -5
```

Penjelasan:

`git rebase main` membuat commit di branch `feature/add-validation` dimulai ulang dari ujung terbaru `main`. History menjadi lebih linear, tetapi commit ID pada feature branch berubah.

### Remote repository lokal

Command:

```bash
cd ..
git init --bare fruit_orders_remote.git
cd fruit_orders_repo

git checkout main
git remote add origin ../fruit_orders_remote.git
git remote -v
git push -u origin main
```

Penjelasan:

Repository bare `fruit_orders_remote.git` dipakai sebagai simulasi GitHub tanpa internet. `git remote add origin` menghubungkan repository lokal ke remote tersebut, lalu `git push -u origin main` mengirim branch `main`.

## 6. Branching Strategy dan Pull Request

GitHub Flow:

- satu branch utama `main` berisi code production-ready
- fitur dibuat di feature branch pendek
- setelah review, feature branch di-merge ke `main`
- cocok untuk tim kecil dan release cepat

GitFlow:

- memakai branch seperti `main`, `develop`, `feature`, `release`, dan `hotfix`
- cocok untuk project dengan beberapa versi production
- lebih formal, tetapi lebih kompleks

GitLab Flow:

- menggabungkan feature-driven development, issue tracking, dan environment branch
- cocok untuk workflow yang punya staging atau production environment

Pull Request:

- permintaan formal untuk merge perubahan dari satu branch ke branch lain
- tempat diskusi, review, dan validasi sebelum perubahan masuk ke branch utama
- membantu menemukan bug, risiko performa, dan pelanggaran standar code

## Reset Playground

Kalau ingin mengulang dari awal, keluar dulu dari folder repo, lalu hapus folder manual playground:

```bash
cd /Users/riodpp/Documents/Freelance/Purwadhika
rm -rf day12/playground_output/manual_git_playground
```

Setelah itu ulangi command dari bagian awal dokumen ini.

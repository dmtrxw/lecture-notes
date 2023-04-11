# Implementation

## Route

Menulis route di REST API tidak perlu menambahkan `action` di path karena `action` sudah
di-representasikan oleh HTTP Method.

-   `GET`: ambil data
-   `POST`: buat data
-   `PUT`: memperbarui data
-   `DELETE`: hapus data

```javascript
router.post('/movies/create-new-movie', MovieController.create) // ❌💀😩
router.post('/movies', MovieController.create) // ✅
```

## Controller

```javascript
/* ... */
class MovieController {
    /* ... */
    static async create(req, res) {
        try {
            const movie = await Movie.create(/* ... */)

            /**
             * Ketika action berhasil, kirim status code dan response yang sesuai.
             *
             * Dalam kasus ini kita pakai 201 (Created) karena kita berhasil
             * membuat data. Kirim juga data yang berhasil dibuat sebagai response
             */
            res.status(201).json(movie)
        } catch (err) {
            next(err)
        }
    }
    /* ... */
}
/* ... */
```

## Error Handler

```javascript
// parameter error handler HARUS 4
function errorHandler(err, req, res, next) {
    /**
     * Ketika action gagal, pastikan dulu errornya karena kesalahan request
     * atau kesalahan server dan kirim response yang sesuai.
     *
     * Kalau kesalahan request, kirim status code 4xx (sesuaikan dengan errornya).
     * Kalau tidak, kirim status code 500 (Internal Server Error)
     */
    if (err.name === 'SequelizeValidationError') {
        // status code: 400
    } else {
        // status code: 500
    }
}
```

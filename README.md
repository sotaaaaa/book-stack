**BOOK STACK DEMO**

```
Test
```

#H1

```javascript
gulp.task('clean', function () {
  return del(['docs/**', 'docs/.*', '!docs'], {
    force: true
  });
});
gulp.task('copy', function () {
  return gulp.src([
      '_book/**/*'
    ])
    .pipe(gulp.dest('docs'));
});
```

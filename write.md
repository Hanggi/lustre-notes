# Linux write behavior


/fs/read_write.c
### vfs_write()

```
write() SYSTEM CALL
└─> __vfs_write()
    └─> file->f_op->write()
        └─> do_sync_write()
            └─> aio_write()
                └─> ldiskfs_file_write()
                    └─> generic_file_aio_write()
                        └─> __generic_file_aio_write()
                            └─> generic_file_buffered_write()
                                └─> generic_perform_write()
                                    └─> a_ops->write_begin()
                                    └─> iov_iter_copy_from_user_atomic()
                                    └─> a_ops->write_end()
```


```

```

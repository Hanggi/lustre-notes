# Linux write behavior


/fs/read_write.c
### vfs_write()

```

    └─> ll_file_write()
        └─> ll_file_aio_write()
            └─> ll_file_write_iter()
                └─> ll_file_io_generic()
                    └─> ll_io_init()
                    └─> cl_io_rw_init()
                    └─> ll_cl_add()
                        └─> list_add()
                    └─> cl_io_loop() --- main io loop!
                        └─> cl_io_iter_init()
                        └─> cl_io_lock()
                        └─> cl_io_start()
                            └─> vol_io_start / osc_io_write_start
                            └─> vvp_io_write_start()::q
                                └─> __generic_file_aio_write
                        └─> cl_io_end()
                            └─> vvp_io_rw_end / lov_io_end / osc_io_end
                        └─> cl_io_unlock()
                        └─> cl_io_iter_fini() --- repeatedly until there is no more io to do.
                    └─> ll_cl_remove()


```

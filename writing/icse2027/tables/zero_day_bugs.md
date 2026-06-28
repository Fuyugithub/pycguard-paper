# Zero-Day Bug Submissions (45 issues across 9 evaluated projects)

| ID | Project | Description | Status |
|---:|---------|-------------|--------|
| 1 | scipy | SuperLUGlobal_dealloc reads value instead of key — all SuperLU memory leaks | Confirmed |
| 2 | scipy | Py_XDECREF on borrowed references in fitpack_sphere when iopt==-1 | Confirmed |
| 3 | scipy | Double free of wrk1/wrk2/iwrk in fitpack_surfit on allocation failure | Fixed |
| 4 | scipy | Double DECREF of ap_t in fitpack_parcur | Confirmed |
| 5 | scipy | PyTuple_SET_ITEM on unchecked NULL args_tuple in DVODE/ZVODE thunks | Confirmed |
| 6 | scipy | Double DECREF of ap_t in fitpack_parcur (resubmitted with reproducer) | Confirmed |
| 7 | scipy | PyTuple_SET_ITEM on unchecked NULL args_tuple in DVODE/ZVODE thunks (resubmitted) | Confirmed |
| 8 | scipy | PyTuple_SetItem mutates shared args_tuple across PROPACK iterations | Confirmed |
| 9 | numpy | DType reference leak in _convert_from_type | Confirmed |
| 10 | numpy | Reference leak in PyUFunc_GenericReduction on CheckOverride error | Fixed |
| 11 | numpy | typecode reference leak in array_setstate on error paths | Fixed |
| 12 | numpy | out reference leak in PyUFunc_Accumulate on type resolution failure | Confirmed |
| 13 | numpy | out reference leak in PyUFunc_Accumulate and PyUFunc_Reduceat on type resolution failure | Fixed |
| 14 | pillow | path_subscript hardcodes slice length to 4 instead of self->count | Fixed |
| 15 | pillow | Use after release of Py_buffer in _prepare_lut_table | Fixed |
| 16 | pillow | Reference leak of seq in set_value_to_item macro on nested sequence error | Fixed |
| 17 | pillow | Reference leak of tag_type in PyImaging_LibTiffEncoderNew | Fixed |
| 18 | pandas | Use after free in int64ToIso when ret_code is non-zero | Confirmed |
| 19 | pandas | NULL pointer dereference in get_long_attr (ujson encoder) | Confirmed |
| 20 | pandas | NULL pointer dereference in total_seconds (ujson encoder) | Confirmed |
| 21 | psutil | Use after free of spi in psutil_cpu_stats | Confirmed |
| 22 | psutil | Stack buffer overflow due to unclamped sdl_nlen in net_if_stats | Confirmed |
| 23 | psutil | Wrong variable checked after PyUnicode_DecodeFSDefault — NULL dereference | Confirmed |
| 24 | ujson | NULL pointer dereference: PyObject_Malloc unchecked in decode_numeric | Confirmed |
| 25 | ujson | Reference leak in object_is_decimal_type on PyPy | Confirmed |
| 26 | ujson | NULL dereference: PyTuple_Pack unchecked in JSONFileToObj | Confirmed |
| 27 | pycurl | Format mismatch: long timeout_ms passed to Py_BuildValue "(i)" | Fixed |
| 28 | pycurl | PY_SSIZE_T_CLEAN defined but int passed to "#" length format in debug_callback | Fixed |
| 29 | pycurl | Reference leak in perform_rb: Py_None leaked twice per call | Fixed |
| 30 | pycurl | PyObject_IsTrue return value -1 not checked in progress and xferinfo callbacks | Fixed |
| 31 | pycurl | Reference leak of error_str on success path in do_multi_info_read | Fixed |
| 32 | pytorch | Py_None reference stolen without Py_INCREF in THPGenerator_reduce | Confirmed |
| 33 | pytorch | THPStream_richcompare casts arbitrary objects to THPStream* without type check | Confirmed |
| 34 | pytorch | Reference leak in autograd Function.apply — setup_context return value never released | Confirmed |
| 35 | tensorflow | FastModuleType tp_setattro crashes on attribute deletion (Py_INCREF on NULL) | Confirmed |
| 36 | tensorflow | SetGetattributeCallback corrupts module state when ParseFunc fails | Confirmed |
| 37 | numpy | Descriptor not restored on error path of array_partition | Confirmed |
| 38 | scipy | PyTuple_New return not NULL checked in dvode_function_thunk | Confirmed |
| 39 | pandas | NULL dereference in Object_getBigNumStringValue (ujson encoder) | Confirmed |
| 40 | pillow | Imaging structures leaked on error paths of _split | Confirmed |
| 41 | psutil | Stale pointer when realloc fails in psutil_gather_inet | Confirmed |
| 42 | ujson | Missing NULL check on PyObject_Malloc in Object_newIntegerFromString | Confirmed |
| 43 | pycurl | PyObject_IsTrue return value -1 not checked in xferinfo_callback | Confirmed |
| 44 | pytorch | Reference leak of result in THPFunction_apply setup_context path | Confirmed |
| 45 | tensorflow | Cascading NULL dereference in AttrsValueIterator next | Confirmed |
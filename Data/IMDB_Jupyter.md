```python
from sqlalchemy.engine import create_engine
import pymysql
pymysql.install_as_MySQLdb()
from urllib.parse import quote_plus
import pandas as pd
```


```python
# Create the sqlalchemy engine and connection
username = "root"
password = "206!!Lbs" 
# password = quote_plus("Myp@ssword!") # Use the quote function if you have special chars in password
db_name = "IMDB"
connection = f"mysql+pymysql://{username}:{password}@localhost/{db_name}"
engine = create_engine(connection)
conn = engine.connect()
```


```python
# Preview the names of all tables 
q = '''SHOW TABLES;'''
pd.read_sql(q, conn)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Tables_in_imdb</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>genres</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ratings</td>
    </tr>
    <tr>
      <th>2</th>
      <td>title_basics</td>
    </tr>
    <tr>
      <th>3</th>
      <td>title_genres</td>
    </tr>
  </tbody>
</table>
</div>



![png](Movies-ERD.png)


```python
# Get a list of tables in the database
tables = engine.table_names()

# Iterate through the tables and describe each one
for table_name in tables:
    query = f"DESCRIBE {table_name}"
    table_description = pd.read_sql_query(query, engine)
    
    # Display the table name and its description
    display(f"Table: {table_name}")
    display(table_description)
```

    C:\Users\benja\AppData\Local\Temp\ipykernel_3564\1311453169.py:2: SADeprecationWarning: The Engine.table_names() method is deprecated and will be removed in a future release.  Please refer to Inspector.get_table_names(). (deprecated since: 1.4)
      tables = engine.table_names()
    


    'Table: genres'



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Field</th>
      <th>Type</th>
      <th>Null</th>
      <th>Key</th>
      <th>Default</th>
      <th>Extra</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>genre_id</td>
      <td>int</td>
      <td>NO</td>
      <td>PRI</td>
      <td>None</td>
      <td>auto_increment</td>
    </tr>
    <tr>
      <th>1</th>
      <td>genre_name</td>
      <td>varchar(45)</td>
      <td>YES</td>
      <td></td>
      <td>None</td>
      <td></td>
    </tr>
  </tbody>
</table>
</div>



    'Table: ratings'



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Field</th>
      <th>Type</th>
      <th>Null</th>
      <th>Key</th>
      <th>Default</th>
      <th>Extra</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>tconst</td>
      <td>int</td>
      <td>NO</td>
      <td>PRI</td>
      <td>None</td>
      <td>auto_increment</td>
    </tr>
    <tr>
      <th>1</th>
      <td>average_rating</td>
      <td>varchar(45)</td>
      <td>YES</td>
      <td></td>
      <td>None</td>
      <td></td>
    </tr>
    <tr>
      <th>2</th>
      <td>number_of_votes</td>
      <td>varchar(45)</td>
      <td>YES</td>
      <td></td>
      <td>None</td>
      <td></td>
    </tr>
    <tr>
      <th>3</th>
      <td>title_basics_tonst</td>
      <td>int</td>
      <td>NO</td>
      <td></td>
      <td>None</td>
      <td></td>
    </tr>
  </tbody>
</table>
</div>



    'Table: title_basics'



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Field</th>
      <th>Type</th>
      <th>Null</th>
      <th>Key</th>
      <th>Default</th>
      <th>Extra</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>tonst</td>
      <td>int</td>
      <td>NO</td>
      <td>PRI</td>
      <td>None</td>
      <td>auto_increment</td>
    </tr>
    <tr>
      <th>1</th>
      <td>primary_title</td>
      <td>varchar(45)</td>
      <td>YES</td>
      <td></td>
      <td>None</td>
      <td></td>
    </tr>
    <tr>
      <th>2</th>
      <td>start_year</td>
      <td>int</td>
      <td>YES</td>
      <td></td>
      <td>None</td>
      <td></td>
    </tr>
    <tr>
      <th>3</th>
      <td>runtime</td>
      <td>varchar(45)</td>
      <td>YES</td>
      <td></td>
      <td>None</td>
      <td></td>
    </tr>
  </tbody>
</table>
</div>



    'Table: title_genres'



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Field</th>
      <th>Type</th>
      <th>Null</th>
      <th>Key</th>
      <th>Default</th>
      <th>Extra</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>tconst</td>
      <td>int</td>
      <td>NO</td>
      <td>PRI</td>
      <td>None</td>
      <td></td>
    </tr>
    <tr>
      <th>1</th>
      <td>genre_id</td>
      <td>int</td>
      <td>NO</td>
      <td>PRI</td>
      <td>None</td>
      <td></td>
    </tr>
  </tbody>
</table>
</div>



```python
# Load the TSV file
tsv_basics_path = 'Data/title.basics.tsv.gz'
df = pd.read_csv(tsv_basics_path, delimiter='\t', low_memory = False)
```


```python
# Load the TSV file
tsv_ratings_path = 'Data/title.ratings.tsv.gz'
df = pd.read_csv(tsv_ratings_path, delimiter='\t', low_memory = False)
```


```python
tsv_basics_path = 'Data/title.basics.tsv.gz'  
tsv_ratings_path = 'Data/title.ratings.tsv.gz'      

# Load TSV files into DataFrames
title_basics = pd.read_csv(tsv_basics_path, delimiter='\t', low_memory = False)
ratings = pd.read_csv(tsv_ratings_path, delimiter='\t', low_memory = False)
```


```python
# Append the title_basics DataFrame to the 'title_basics' table
title_basics.to_sql('title_basics', engine, if_exists='append', index=False)

# Append the ratings DataFrame to the 'ratings' table
ratings.to_sql('ratings', engine, if_exists='append', index=False)
```


    ---------------------------------------------------------------------------

    OperationalError                          Traceback (most recent call last)

    File ~\anaconda3\envs\dojo-env\lib\site-packages\sqlalchemy\engine\base.py:1890, in Connection._execute_context(self, dialect, constructor, statement, parameters, execution_options, *args, **kw)
       1889     if not evt_handled:
    -> 1890         self.dialect.do_executemany(
       1891             cursor, statement, parameters, context
       1892         )
       1893 elif not parameters and context.no_parameters:
    

    File ~\anaconda3\envs\dojo-env\lib\site-packages\sqlalchemy\dialects\mysql\mysqldb.py:180, in MySQLDialect_mysqldb.do_executemany(self, cursor, statement, parameters, context)
        179 def do_executemany(self, cursor, statement, parameters, context=None):
    --> 180     rowcount = cursor.executemany(statement, parameters)
        181     if context is not None:
    

    File ~\anaconda3\envs\dojo-env\lib\site-packages\pymysql\cursors.py:187, in Cursor.executemany(self, query, args)
        186     assert q_values[0] == "(" and q_values[-1] == ")"
    --> 187     return self._do_execute_many(
        188         q_prefix,
        189         q_values,
        190         q_postfix,
        191         args,
        192         self.max_stmt_length,
        193         self._get_db().encoding,
        194     )
        196 self.rowcount = sum(self.execute(query, arg) for arg in args)
    

    File ~\anaconda3\envs\dojo-env\lib\site-packages\pymysql\cursors.py:220, in Cursor._do_execute_many(self, prefix, values, postfix, args, max_stmt_length, encoding)
        219 if len(sql) + len(v) + len(postfix) + 1 > max_stmt_length:
    --> 220     rows += self.execute(sql + postfix)
        221     sql = bytearray(prefix)
    

    File ~\anaconda3\envs\dojo-env\lib\site-packages\pymysql\cursors.py:158, in Cursor.execute(self, query, args)
        156 query = self.mogrify(query, args)
    --> 158 result = self._query(query)
        159 self._executed = query
    

    File ~\anaconda3\envs\dojo-env\lib\site-packages\pymysql\cursors.py:325, in Cursor._query(self, q)
        324 self._clear_result()
    --> 325 conn.query(q)
        326 self._do_get_result()
    

    File ~\anaconda3\envs\dojo-env\lib\site-packages\pymysql\connections.py:549, in Connection.query(self, sql, unbuffered)
        548 self._execute_command(COMMAND.COM_QUERY, sql)
    --> 549 self._affected_rows = self._read_query_result(unbuffered=unbuffered)
        550 return self._affected_rows
    

    File ~\anaconda3\envs\dojo-env\lib\site-packages\pymysql\connections.py:779, in Connection._read_query_result(self, unbuffered)
        778     result = MySQLResult(self)
    --> 779     result.read()
        780 self._result = result
    

    File ~\anaconda3\envs\dojo-env\lib\site-packages\pymysql\connections.py:1157, in MySQLResult.read(self)
       1156 try:
    -> 1157     first_packet = self.connection._read_packet()
       1159     if first_packet.is_ok_packet():
    

    File ~\anaconda3\envs\dojo-env\lib\site-packages\pymysql\connections.py:729, in Connection._read_packet(self, packet_type)
        728         self._result.unbuffered_active = False
    --> 729     packet.raise_for_error()
        730 return packet
    

    File ~\anaconda3\envs\dojo-env\lib\site-packages\pymysql\protocol.py:221, in MysqlPacket.raise_for_error(self)
        220     print("errno =", errno)
    --> 221 err.raise_mysql_exception(self._data)
    

    File ~\anaconda3\envs\dojo-env\lib\site-packages\pymysql\err.py:143, in raise_mysql_exception(data)
        142     errorclass = InternalError if errno < 1000 else OperationalError
    --> 143 raise errorclass(errno, errval)
    

    OperationalError: (1054, "Unknown column 'tconst' in 'field list'")

    
    The above exception was the direct cause of the following exception:
    

    OperationalError                          Traceback (most recent call last)

    Cell In[24], line 2
          1 # Append the title_basics DataFrame to the 'title_basics' table
    ----> 2 title_basics.to_sql('title_basics', engine, if_exists='append', index=False)
          4 # Append the ratings DataFrame to the 'ratings' table
          5 ratings.to_sql('ratings', engine, if_exists='append', index=False)
    

    File ~\anaconda3\envs\dojo-env\lib\site-packages\pandas\core\generic.py:2987, in NDFrame.to_sql(self, name, con, schema, if_exists, index, index_label, chunksize, dtype, method)
       2830 """
       2831 Write records stored in a DataFrame to a SQL database.
       2832 
       (...)
       2983 [(1,), (None,), (2,)]
       2984 """  # noqa:E501
       2985 from pandas.io import sql
    -> 2987 return sql.to_sql(
       2988     self,
       2989     name,
       2990     con,
       2991     schema=schema,
       2992     if_exists=if_exists,
       2993     index=index,
       2994     index_label=index_label,
       2995     chunksize=chunksize,
       2996     dtype=dtype,
       2997     method=method,
       2998 )
    

    File ~\anaconda3\envs\dojo-env\lib\site-packages\pandas\io\sql.py:695, in to_sql(frame, name, con, schema, if_exists, index, index_label, chunksize, dtype, method, engine, **engine_kwargs)
        690 elif not isinstance(frame, DataFrame):
        691     raise NotImplementedError(
        692         "'frame' argument should be either a Series or a DataFrame"
        693     )
    --> 695 return pandas_sql.to_sql(
        696     frame,
        697     name,
        698     if_exists=if_exists,
        699     index=index,
        700     index_label=index_label,
        701     schema=schema,
        702     chunksize=chunksize,
        703     dtype=dtype,
        704     method=method,
        705     engine=engine,
        706     **engine_kwargs,
        707 )
    

    File ~\anaconda3\envs\dojo-env\lib\site-packages\pandas\io\sql.py:1738, in SQLDatabase.to_sql(self, frame, name, if_exists, index, index_label, schema, chunksize, dtype, method, engine, **engine_kwargs)
       1726 sql_engine = get_engine(engine)
       1728 table = self.prep_table(
       1729     frame=frame,
       1730     name=name,
       (...)
       1735     dtype=dtype,
       1736 )
    -> 1738 total_inserted = sql_engine.insert_records(
       1739     table=table,
       1740     con=self.connectable,
       1741     frame=frame,
       1742     name=name,
       1743     index=index,
       1744     schema=schema,
       1745     chunksize=chunksize,
       1746     method=method,
       1747     **engine_kwargs,
       1748 )
       1750 self.check_case_sensitive(name=name, schema=schema)
       1751 return total_inserted
    

    File ~\anaconda3\envs\dojo-env\lib\site-packages\pandas\io\sql.py:1335, in SQLAlchemyEngine.insert_records(self, table, con, frame, name, index, schema, chunksize, method, **engine_kwargs)
       1333     raise ValueError("inf cannot be used with MySQL") from err
       1334 else:
    -> 1335     raise err
    

    File ~\anaconda3\envs\dojo-env\lib\site-packages\pandas\io\sql.py:1325, in SQLAlchemyEngine.insert_records(self, table, con, frame, name, index, schema, chunksize, method, **engine_kwargs)
       1322 from sqlalchemy import exc
       1324 try:
    -> 1325     return table.insert(chunksize=chunksize, method=method)
       1326 except exc.SQLAlchemyError as err:
       1327     # GH34431
       1328     # https://stackoverflow.com/a/67358288/6067848
       1329     msg = r"""(\(1054, "Unknown column 'inf(e0)?' in 'field list'"\))(?#
       1330     )|inf can not be used with MySQL"""
    

    File ~\anaconda3\envs\dojo-env\lib\site-packages\pandas\io\sql.py:946, in SQLTable.insert(self, chunksize, method)
        943     break
        945 chunk_iter = zip(*(arr[start_i:end_i] for arr in data_list))
    --> 946 num_inserted = exec_insert(conn, keys, chunk_iter)
        947 # GH 46891
        948 if is_integer(num_inserted):
    

    File ~\anaconda3\envs\dojo-env\lib\site-packages\pandas\io\sql.py:853, in SQLTable._execute_insert(self, conn, keys, data_iter)
        841 """
        842 Execute SQL statement inserting data
        843 
       (...)
        850    Each item contains a list of values to be inserted
        851 """
        852 data = [dict(zip(keys, row)) for row in data_iter]
    --> 853 result = conn.execute(self.table.insert(), data)
        854 return result.rowcount
    

    File ~\anaconda3\envs\dojo-env\lib\site-packages\sqlalchemy\engine\base.py:1385, in Connection.execute(self, statement, *multiparams, **params)
       1381     util.raise_(
       1382         exc.ObjectNotExecutableError(statement), replace_context=err
       1383     )
       1384 else:
    -> 1385     return meth(self, multiparams, params, _EMPTY_EXECUTION_OPTS)
    

    File ~\anaconda3\envs\dojo-env\lib\site-packages\sqlalchemy\sql\elements.py:334, in ClauseElement._execute_on_connection(self, connection, multiparams, params, execution_options, _force)
        330 def _execute_on_connection(
        331     self, connection, multiparams, params, execution_options, _force=False
        332 ):
        333     if _force or self.supports_execution:
    --> 334         return connection._execute_clauseelement(
        335             self, multiparams, params, execution_options
        336         )
        337     else:
        338         raise exc.ObjectNotExecutableError(self)
    

    File ~\anaconda3\envs\dojo-env\lib\site-packages\sqlalchemy\engine\base.py:1577, in Connection._execute_clauseelement(self, elem, multiparams, params, execution_options)
       1565 compiled_cache = execution_options.get(
       1566     "compiled_cache", self.engine._compiled_cache
       1567 )
       1569 compiled_sql, extracted_params, cache_hit = elem._compile_w_cache(
       1570     dialect=dialect,
       1571     compiled_cache=compiled_cache,
       (...)
       1575     linting=self.dialect.compiler_linting | compiler.WARN_LINTING,
       1576 )
    -> 1577 ret = self._execute_context(
       1578     dialect,
       1579     dialect.execution_ctx_cls._init_compiled,
       1580     compiled_sql,
       1581     distilled_params,
       1582     execution_options,
       1583     compiled_sql,
       1584     distilled_params,
       1585     elem,
       1586     extracted_params,
       1587     cache_hit=cache_hit,
       1588 )
       1589 if has_events:
       1590     self.dispatch.after_execute(
       1591         self,
       1592         elem,
       (...)
       1596         ret,
       1597     )
    

    File ~\anaconda3\envs\dojo-env\lib\site-packages\sqlalchemy\engine\base.py:1953, in Connection._execute_context(self, dialect, constructor, statement, parameters, execution_options, *args, **kw)
       1950             branched.close()
       1952 except BaseException as e:
    -> 1953     self._handle_dbapi_exception(
       1954         e, statement, parameters, cursor, context
       1955     )
       1957 return result
    

    File ~\anaconda3\envs\dojo-env\lib\site-packages\sqlalchemy\engine\base.py:2134, in Connection._handle_dbapi_exception(self, e, statement, parameters, cursor, context)
       2132     util.raise_(newraise, with_traceback=exc_info[2], from_=e)
       2133 elif should_wrap:
    -> 2134     util.raise_(
       2135         sqlalchemy_exception, with_traceback=exc_info[2], from_=e
       2136     )
       2137 else:
       2138     util.raise_(exc_info[1], with_traceback=exc_info[2])
    

    File ~\anaconda3\envs\dojo-env\lib\site-packages\sqlalchemy\util\compat.py:211, in raise_(***failed resolving arguments***)
        208     exception.__cause__ = replace_context
        210 try:
    --> 211     raise exception
        212 finally:
        213     # credit to
        214     # https://cosmicpercolator.com/2016/01/13/exception-leaks-in-python-2-and-3/
        215     # as the __traceback__ object creates a cycle
        216     del exception, replace_context, from_, with_traceback
    

    File ~\anaconda3\envs\dojo-env\lib\site-packages\sqlalchemy\engine\base.py:1890, in Connection._execute_context(self, dialect, constructor, statement, parameters, execution_options, *args, **kw)
       1888                 break
       1889     if not evt_handled:
    -> 1890         self.dialect.do_executemany(
       1891             cursor, statement, parameters, context
       1892         )
       1893 elif not parameters and context.no_parameters:
       1894     if self.dialect._has_events:
    

    File ~\anaconda3\envs\dojo-env\lib\site-packages\sqlalchemy\dialects\mysql\mysqldb.py:180, in MySQLDialect_mysqldb.do_executemany(self, cursor, statement, parameters, context)
        179 def do_executemany(self, cursor, statement, parameters, context=None):
    --> 180     rowcount = cursor.executemany(statement, parameters)
        181     if context is not None:
        182         context._rowcount = rowcount
    

    File ~\anaconda3\envs\dojo-env\lib\site-packages\pymysql\cursors.py:187, in Cursor.executemany(self, query, args)
        185     q_postfix = m.group(3) or ""
        186     assert q_values[0] == "(" and q_values[-1] == ")"
    --> 187     return self._do_execute_many(
        188         q_prefix,
        189         q_values,
        190         q_postfix,
        191         args,
        192         self.max_stmt_length,
        193         self._get_db().encoding,
        194     )
        196 self.rowcount = sum(self.execute(query, arg) for arg in args)
        197 return self.rowcount
    

    File ~\anaconda3\envs\dojo-env\lib\site-packages\pymysql\cursors.py:220, in Cursor._do_execute_many(self, prefix, values, postfix, args, max_stmt_length, encoding)
        218     v = v.encode(encoding, "surrogateescape")
        219 if len(sql) + len(v) + len(postfix) + 1 > max_stmt_length:
    --> 220     rows += self.execute(sql + postfix)
        221     sql = bytearray(prefix)
        222 else:
    

    File ~\anaconda3\envs\dojo-env\lib\site-packages\pymysql\cursors.py:158, in Cursor.execute(self, query, args)
        154     pass
        156 query = self.mogrify(query, args)
    --> 158 result = self._query(query)
        159 self._executed = query
        160 return result
    

    File ~\anaconda3\envs\dojo-env\lib\site-packages\pymysql\cursors.py:325, in Cursor._query(self, q)
        323 conn = self._get_db()
        324 self._clear_result()
    --> 325 conn.query(q)
        326 self._do_get_result()
        327 return self.rowcount
    

    File ~\anaconda3\envs\dojo-env\lib\site-packages\pymysql\connections.py:549, in Connection.query(self, sql, unbuffered)
        547     sql = sql.encode(self.encoding, "surrogateescape")
        548 self._execute_command(COMMAND.COM_QUERY, sql)
    --> 549 self._affected_rows = self._read_query_result(unbuffered=unbuffered)
        550 return self._affected_rows
    

    File ~\anaconda3\envs\dojo-env\lib\site-packages\pymysql\connections.py:779, in Connection._read_query_result(self, unbuffered)
        777 else:
        778     result = MySQLResult(self)
    --> 779     result.read()
        780 self._result = result
        781 if result.server_status is not None:
    

    File ~\anaconda3\envs\dojo-env\lib\site-packages\pymysql\connections.py:1157, in MySQLResult.read(self)
       1155 def read(self):
       1156     try:
    -> 1157         first_packet = self.connection._read_packet()
       1159         if first_packet.is_ok_packet():
       1160             self._read_ok_packet(first_packet)
    

    File ~\anaconda3\envs\dojo-env\lib\site-packages\pymysql\connections.py:729, in Connection._read_packet(self, packet_type)
        727     if self._result is not None and self._result.unbuffered_active is True:
        728         self._result.unbuffered_active = False
    --> 729     packet.raise_for_error()
        730 return packet
    

    File ~\anaconda3\envs\dojo-env\lib\site-packages\pymysql\protocol.py:221, in MysqlPacket.raise_for_error(self)
        219 if DEBUG:
        220     print("errno =", errno)
    --> 221 err.raise_mysql_exception(self._data)
    

    File ~\anaconda3\envs\dojo-env\lib\site-packages\pymysql\err.py:143, in raise_mysql_exception(data)
        141 if errorclass is None:
        142     errorclass = InternalError if errno < 1000 else OperationalError
    --> 143 raise errorclass(errno, errval)
    

    OperationalError: (pymysql.err.OperationalError) (1054, "Unknown column 'tconst' in 'field list'")
    [SQL: INSERT INTO title_basics (tconst, `titleType`, `primaryTitle`, `originalTitle`, `isAdult`, `startYear`, `endYear`, `runtimeMinutes`, genres) VALUES (%(tconst)s, %(titleType)s, %(primaryTitle)s, %(originalTitle)s, %(isAdult)s, %(startYear)s, %(endYear)s, %(runtimeMinutes)s, %(genres)s)]
    [parameters: ({'tconst': 'tt0000001', 'titleType': 'short', 'primaryTitle': 'Carmencita', 'originalTitle': 'Carmencita', 'isAdult': '0', 'startYear': '1894', 'endYear': '\\N', 'runtimeMinutes': '1', 'genres': 'Documentary,Short'}, {'tconst': 'tt0000002', 'titleType': 'short', 'primaryTitle': 'Le clown et ses chiens', 'originalTitle': 'Le clown et ses chiens', 'isAdult': '0', 'startYear': '1892', 'endYear': '\\N', 'runtimeMinutes': '5', 'genres': 'Animation,Short'}, {'tconst': 'tt0000003', 'titleType': 'short', 'primaryTitle': 'Pauvre Pierrot', 'originalTitle': 'Pauvre Pierrot', 'isAdult': '0', 'startYear': '1892', 'endYear': '\\N', 'runtimeMinutes': '4', 'genres': 'Animation,Comedy,Romance'}, {'tconst': 'tt0000004', 'titleType': 'short', 'primaryTitle': 'Un bon bock', 'originalTitle': 'Un bon bock', 'isAdult': '0', 'startYear': '1892', 'endYear': '\\N', 'runtimeMinutes': '12', 'genres': 'Animation,Short'}, {'tconst': 'tt0000005', 'titleType': 'short', 'primaryTitle': 'Blacksmith Scene', 'originalTitle': 'Blacksmith Scene', 'isAdult': '0', 'startYear': '1893', 'endYear': '\\N', 'runtimeMinutes': '1', 'genres': 'Comedy,Short'}, {'tconst': 'tt0000006', 'titleType': 'short', 'primaryTitle': 'Chinese Opium Den', 'originalTitle': 'Chinese Opium Den', 'isAdult': '0', 'startYear': '1894', 'endYear': '\\N', 'runtimeMinutes': '1', 'genres': 'Short'}, {'tconst': 'tt0000007', 'titleType': 'short', 'primaryTitle': 'Corbett and Courtney Before the Kinetograph', 'originalTitle': 'Corbett and Courtney Before the Kinetograph', 'isAdult': '0', 'startYear': '1894', 'endYear': '\\N', 'runtimeMinutes': '1', 'genres': 'Short,Sport'}, {'tconst': 'tt0000008', 'titleType': 'short', 'primaryTitle': 'Edison Kinetoscopic Record of a Sneeze', 'originalTitle': 'Edison Kinetoscopic Record of a Sneeze', 'isAdult': '0', 'startYear': '1894', 'endYear': '\\N', 'runtimeMinutes': '1', 'genres': 'Documentary,Short'}  ... displaying 10 of 10017011 total bound parameter sets ...  {'tconst': 'tt9916856', 'titleType': 'short', 'primaryTitle': 'The Wind', 'originalTitle': 'The Wind', 'isAdult': '0', 'startYear': '2015', 'endYear': '\\N', 'runtimeMinutes': '27', 'genres': 'Short'}, {'tconst': 'tt9916880', 'titleType': 'tvEpisode', 'primaryTitle': 'Horrid Henry Knows It All', 'originalTitle': 'Horrid Henry Knows It All', 'isAdult': '0', 'startYear': '2014', 'endYear': '\\N', 'runtimeMinutes': '10', 'genres': 'Adventure,Animation,Comedy'})]
    (Background on this error at: https://sqlalche.me/e/14/e3q8)


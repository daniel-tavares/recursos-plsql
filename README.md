# RESTORE OTIMIZADO POSTGRESQL

/opt/PostgreSQL/10/bin/pg_restore --host localhost --port 5434 --username exmart   --role exmart -j 4 --dbname rtdpjlite_test /u01/bkp/sp_2osasco.bkp


# PLSL

## EXCLUIR TODAS AS TABELAS

    begin
     for deleta in (select table_name, 'DROP TABLE '||table_name||' cascade constraints' AS dropar from user_tables) loop
     BEGIN
      EXECUTE IMMEDIATE deleta.dropar;
      dbms_output.put_line('DROP TABLE '||deleta.table_name||' cascade constraints;');
      EXCEPTION WHEN OTHERS THEN
      dbms_output.put_line('Erro ao tentar dropar a tabela:'||deleta.table_name);
     END;
     end loop;
    end;

## EXCLUIR TODAS AS SEQUENCIAS

    begin
     for deleta in (select sequence_name, 'DROP SEQUENCE '||sequence_name||' ' AS dropar from user_sequences) loop
     BEGIN
      EXECUTE IMMEDIATE deleta.dropar;
      dbms_output.put_line('DROP SEQUENCE '||deleta.sequence_name||' ;');
      EXCEPTION WHEN OTHERS THEN
      dbms_output.put_line('Erro ao tentar dropar a tabela:'||deleta.sequence_name);
     END;
     end loop;
    end;


## COMANDOS ADMIN
- SELECT table_name from user_tables;
- SELECT owner||'.' ||table_name Objeto , privilege FROM dba_tab_privs WHERE grantee = upper('PORTAL')
- GRANT ALL ON  nomeTabela TO nomeUsuario;


## CURSOR, FOR e IF 

        DECLARE
            c_id NUMBER;
            existe NUMBER;
            valor NUMBER;
          CURSOR ARQUIVO_ECF IS
              SELECT   re.tra_ted ted
              FROM ecf.tab_ecf_resumo_arquivo re;
          R_ARQUIVO     ARQUIVO_ECF % rowtype;


        BEGIN
           existe:=0;
           valor:=0;
           FOR i IN 125 ..2000 LOOP
             FOR R_ARQUIVO IN ARQUIVO_ECF LOOP
                IF i =R_ARQUIVO.ted THEN
                   existe:=1;
                END IF ;
             END LOOP ;
            IF ( existe =1) THEN
                  existe:=0;
             ELSE
                dbms_output.put_line( i );
             END IF ;
           END LOOP ;
        END ;


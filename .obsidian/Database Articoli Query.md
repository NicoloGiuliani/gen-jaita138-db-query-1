### Livello 1: Interrogazioni di Base
```sql
-- 1. Recupera tutti gli utenti registrati nel sistema.
select *
from utenti

-- 2. Recupera il nome, cognome e email di tutti gli utenti iscritti dopo il 1° marzo 2024.
select nome, cognome, email
from utenti u
where u.data_iscrizione>='2024-03-01'

-- 3. Recupera tutti gli articoli insieme al nome e cognome dell'autore.
select a.*, u.nome, u.cognome
from articoli a
join utenti u
on a.id_utente = u.id_utente

-- 4. Recupera i titoli degli articoli pubblicati nel mese di maggio 2024.
select a.titolo
from articoli a
where month(a.data_pubblicazione)=05 and year(a.data_pubblicazione)=2024

-- 5. Recupera le prime 5 categorie ordinate alfabeticamente per nome.
select *
from categorie c
order by nome_categoria
limit 5

-- 6. Recupera gli articoli che appartengono alla categoria 'Tecnologia'.
select *
from articoli_categorie ac
where id_categoria =1

-- 7. Recupera le email degli utenti che hanno scritto almeno un articolo.
select u.email
from utenti u
join articoli a
on a.id_articolo = u.id_utente
where a.id_utente >=1

-- 8. Recupera tutti gli articoli pubblicati tra il 1° giugno 2024 e il 31 agosto 2024.
select *
from articoli a
where data_pubblicazione between '2024-06-01' and '2024-08-31'

-- 9. Recupera i dettagli delle categorie associate all'articolo con `id_articolo` = 10.
select *
from articoli_categorie ac
join articoli a
on ac.id_articolo = a.id_articolo
where ac.id_articolo = 10

-- 10. Recupera i nomi degli utenti ordinati per data di iscrizione più recente.
select u.nome, u.data_iscrizione
from utenti u
order by u.data_iscrizione desc
```

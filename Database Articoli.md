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
join utenti u on a.id_utente = u.id_utente 


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
select a.*
from articoli a
join articoli_categorie ac on a.id_articolo = ac.id_articolo
join categorie c on ac.id_categoria = c.id_categoria
where c.nome_categoria like 'Tecnologia'

-- 7. Recupera le email degli utenti che hanno scritto almeno un articolo.
select distinct u. email
from utenti u
join articoli a on u.id_utente = a.id_utente

-- 8. Recupera tutti gli articoli pubblicati tra il 1° giugno 2024 e il 31 agosto 2024.
select *
from articoli a 
where data_pubblicazione between '2024-06-01' and '2024-08-31'

-- 9. Recupera i dettagli delle categorie associate all'articolo con `id_articolo` = 10.
select c.*
from articoli_categorie ac 
join categorie c on ac.id_articolo = c.id_articolo
where ac.id_articolo = 10 

-- 10. Recupera i nomi degli utenti ordinati per data di iscrizione più recente.
select u.nome, u.data_iscrizione 
from utenti u 
order by u.data_iscrizione desc
```

### Livello 2: Interrogazioni intermedie
```sql
-- 1. Conta il numero di articoli scritti da ogni utente.
select u.*, count(a.id_articolo) as 'numero_articoli'
from utenti u
join articoli a on u.id_utente = a.id_utente
group by u.id_utente

-- 2. Trova la categoria con il maggior numero di articoli associati.
select c.id_categoria, count(ac.id_articolo) as n_articoli
from categorie c
join articoli_categorie ac on c.id_categoria = ac.id_categoria
group by c.id_categoria
order by n_articoli desc
limit 1

-- 3. Recupera gli utenti che hanno scritto più di 2 articoli.
select u.*, count(a.id_articolo) as n_articoli
from utenti u
join articoli a on u.id_utente = a.id_utente
group by a.id_utente
having n_articoli > 2

-- 4. Calcola la data di pubblicazione più recente per ogni categoria.
select ac.id_categoria, max(a.data_pubblicazione)
from articoli_categorie ac
join articoli a on ac.id_articolo = a.id_articolo
group by ac.id_categoria

-- 5. Trova il numero medio di articoli per utente.

-- 6. Recupera le categorie che hanno almeno 3 articoli associati.
select c.*, count(ac.id_articolo) as n_articoli
from categorie c
join articoli_categorie ac on c.id_categoria = ac.id_categoria
group by c.id_categoria
having n_articoli >= 3
order by n_articoli desc

-- 7. Calcola il totale degli articoli pubblicati per ogni mese del 2024.
select month(a.data_pubblicazione) as mese_di_pubblicazione, count(ac.id_articolo) as n_articoli
from articoli a
join articoli_categorie ac on a.id_articolo = ac.id_articolo
group by mese_di_pubblicazione
order by mese_di_pubblicazione

-- 8. Trova l'utente che ha la data di iscrizione più antica.
select u.*
from utenti u
order by u.data_iscrizione
limit 1

-- 9. Recupera le categorie e il numero di articoli associati a ciascuna, ordinati dal più al meno.
select c.*, count(ac.id_articolo) as n_articoli
from categorie c
join articoli_categorie ac on c.id_categoria = ac.id_categoria
group by c.id_categoria
order by n_articoli desc

-- 10. Calcola il numero totale di articoli pubblicati da utenti iscritti nel 2024.
select count(a.id_articolo) as numero_articoli_pubblicati
from utenti u
join articoli a on u.id_utente = a.id_utente
where year(u.data_iscrizione) = 2024;
```

### Livello 3: Interrogazioni bonus
```sql
-- 1. Recupera gli utenti che non hanno scritto nessun articolo.
select u.*
from utenti u 
left join articoli a on u.id_utente = a.id_utente
where a.id_articolo is null

-- 2. Trova le categorie che non hanno alcun articolo associato.
select c.*
from categorie c 
left join articoli_categorie ac on c.id_categoria = ac.id_categoria 
where ac.id_articolo is null 

-- 3. Trova le categorie che contengono articoli scritti da utenti iscritti prima del 2024.
select distinct c.*
from utenti u 
join articoli a on u.id_utente = a.id_utente 
join articoli_categorie ac on a.id_articolo = ac.id_articolo 
join categorie c on ac.id_categoria = c.id_categoria 
where u.data_iscrizione < '2024-01-01'
order by c.id_categoria 

-- 4. Recupera le categorie che sono associate ad almeno 3 articoli pubblicati nel 2024.
select c.*, count(*) as n_articoli
from categorie c
join articoli_categorie ac on c.id_categoria = ac.id_categoria
join articoli a on ac.id_articolo = a.id_articolo
where year(a.data_pubblicazione)= 2024
group by c.id_categoria
having n_articoli>=3
```
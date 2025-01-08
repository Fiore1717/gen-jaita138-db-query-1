-- Livello 1: Interrogazioni di Base


-- Recupera tutti gli utenti registrati nel sistema.

select *
from utenti u 

-- -------------------------------------------------------------------------------------------


-- Recupera il nome, cognome e email di tutti gli utenti iscritti dopo il 1° marzo 2024.

select *
from utenti u 
where data_iscrizione > '2024-03-01'

-- ---------------------------------------------------------------------------------------------


-- Recupera tutti gli articoli insieme al nome e cognome dell'autore.

select u.nome,u.cognome, a.id_articolo,a.titolo 
from utenti u 
join articoli a 
on u.id_utente = a.id_utente

-- -----------------------------------------------------------------------------------------------


-- Recupera i titoli degli articoli pubblicati nel mese di maggio 2024.

select titolo 
from articoli a 
where data_pubblicazione > '2024-04-30' and data_pubblicazione <'2024-06-01'

-- ------------------------------------------------------------------------------------------------

-- Recupera le prime 5 categorie ordinate alfabeticamente per nome.

select nome_categoria 
from categorie c 
order by nome_categoria 
limit 5

-- ------------------------------------------------------------------------------------------------

-- Recupera gli articoli che appartengono alla categoria 'Tecnologia'.

select *
from articoli a 
join categorie c 
where c.nome_categoria = "Tecnologia"

-- -------------------------------------------------------------------------------------------------

-- Recupera le email degli utenti che hanno scritto almeno un articolo.

select distinct u.nome, u.cognome, u.email 
from utenti u 
join articoli a 
on u.id_utente = a.id_utente

-- ----------------------------------------------------------------------------------------------------

-- Recupera tutti gli articoli pubblicati tra il 1° giugno 2024 e il 31 agosto 2024.

select titolo 
from articoli a 
where data_pubblicazione > '2024-05-31' and data_pubblicazione <'2024-09-01'
-- ------------------------------------------------------------------------------------------------------

-- Recupera i dettagli delle categorie associate all'articolo con id_articolo = 10.

select a.id_articolo ,c.id_categoria ,c.nome_categoria ,c.descrizione 
from articoli a 
join categorie c 
where id_articolo = 10

-- -------------------------------------------------------------------------------------------------------


-- Recupera i nomi degli utenti ordinati per data di iscrizione più recente.

select nome, data_iscrizione 
from utenti u 
order by data_iscrizione desc

-- -----------------------------------------------------------------------------------------------------------
-- 
-- 
-- -------------------------------------------------------------------------------------------------------------

-- Livello 2: Interrogazioni Intermedie

-- Conta il numero di articoli scritti da ogni utente.


select u.nome ,u.cognome,count(id_articolo) as numero_articoli_scritti
from utenti u 
join articoli a on u.id_utente = a.id_utente
group by u.id_utente, u.nome, u.cognome

-- -------------------------------------------------------------------------------------------------------

-- Recupera gli utenti che hanno scritto più di 2 articoli.

select u.nome ,u.cognome,count(id_articolo) as numero_articoli_scritti
from utenti u 
join articoli a on u.id_utente = a.id_utente
group by u.id_utente, u.nome, u.cognome
having count(a.id_articolo) > 2
  
-- -------------------------------------------------------------------------------------------------------

   
-- Trova la categoria con il maggior numero di articoli associati.

select   c.nome_categoria,count(ac.id_articolo) as numero_articoli
from     categorie c
join     articoli_categorie ac on c.id_categoria = ac.id_categoria
group by c.id_categoria
order by numero_articoli desc
limit 1;

-- -------------------------------------------------------------------------------------------------------

-- Recupera le categorie che hanno almeno 3 articoli associati.

select   c.nome_categoria,count(ac.id_articolo) as numero_articoli
from     categorie c
join     articoli_categorie ac on c.id_categoria = ac.id_categoria
group by c.id_categoria
having count(ac.id_articolo) >= 3

-- -------------------------------------------------------------------------------------------------------

-- Recupera le categorie e il numero di articoli associati a ciascuna, ordinati dal più al meno.

select   c.nome_categoria,count(ac.id_articolo) as numero_articoli_associati
from     categorie c
join     articoli_categorie ac on c.id_categoria = ac.id_categoria
group by c.id_categoria, c.nome_categoria
order by  numero_articoli_associati desc;

# postgres


DROP TABLE IF EXISTS scorecard;

CREATE TABLE scorecard(
	id INTEGER,
	ind_risk CHAR,
	man_risk CHAR,
	fin_flex CHAR,
	credibility CHAR,
	competitiveness CHAR,
	oper_risk CHAR,
 	class VARCHAR(2)
	);
	
\COPY scorecard FROM 'C:\Users\alikh\Desktop\Database\hw8\ids.csv' WITH (FORMAT csv);

DROP TABLE IF EXISTS level;
CREATE TABLE level (
	risk_level TEXT,
	risk_score INTEGER PRIMARY KEY
	);

INSERT INTO level VALUES
('High', 1),
('Medium-High', 2),
('Medium', 3),
('Low', 4) ;
	

DROP TABLE IF EXISTS risk;

CREATE TABLE risk AS
	SELECT id,  
		SUM ((CASE WHEN ind_risk='N' THEN 1 ELSE 0 END) +
		    (CASE WHEN man_risk='N' THEN 1 ELSE 0 END) +
		    (CASE WHEN fin_flex='N' THEN 1 ELSE 0 END) + 
		    (CASE WHEN credibility='N' THEN 1 ELSE 0 END) +
		    (CASE WHEN competitiveness='N' THEN 1 ELSE 0 END) +
		    (CASE WHEN oper_risk='N' THEN 1 ELSE 0 END) )  AS r_l
		 
		FROM scorecard
		GROUP BY id
		ORDER BY id ASC
		;




SELECT (CASE WHEN r_l > 5 THEN risk_score='1' END),
	 (CASE WHEN r_l < 5 THEN '2' END),
	SUM (CASE WHEN r_l < 4 THEN '2' END),
	SUM (CASE WHEN r_l <= 2 THEN '1' END)
	FROM risk r 
	
;

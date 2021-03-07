# mysql_sys.x-statements_with_errors_or_warnings-

SELECT 
    `performance_schema`.`events_statements_summary_by_digest`.`DIGEST_TEXT` AS `query`,
    `performance_schema`.`events_statements_summary_by_digest`.`SCHEMA_NAME` AS `db`,
    `performance_schema`.`events_statements_summary_by_digest`.`COUNT_STAR` AS `exec_count`,
    `performance_schema`.`events_statements_summary_by_digest`.`SUM_ERRORS` AS `errors`,
    (IFNULL((`performance_schema`.`events_statements_summary_by_digest`.`SUM_ERRORS` / NULLIF(`performance_schema`.`events_statements_summary_by_digest`.`COUNT_STAR`,
                    0)),
            0) * 100) AS `error_pct`,
    `performance_schema`.`events_statements_summary_by_digest`.`SUM_WARNINGS` AS `warnings`,
    (IFNULL((`performance_schema`.`events_statements_summary_by_digest`.`SUM_WARNINGS` / NULLIF(`performance_schema`.`events_statements_summary_by_digest`.`COUNT_STAR`,
                    0)),
            0) * 100) AS `warning_pct`,
    `performance_schema`.`events_statements_summary_by_digest`.`FIRST_SEEN` AS `first_seen`,
    `performance_schema`.`events_statements_summary_by_digest`.`LAST_SEEN` AS `last_seen`,
    `performance_schema`.`events_statements_summary_by_digest`.`DIGEST` AS `digest`
FROM
    `performance_schema`.`events_statements_summary_by_digest`
WHERE
    ((`performance_schema`.`events_statements_summary_by_digest`.`SUM_ERRORS` > 0)
        OR (`performance_schema`.`events_statements_summary_by_digest`.`SUM_WARNINGS` > 0))
ORDER BY `performance_schema`.`events_statements_summary_by_digest`.`SUM_ERRORS` DESC , `performance_schema`.`events_statements_summary_by_digest`.`SUM_WARNINGS` DESC

function calculateTeamFinanceReport(salaries, team) {
  const accum = new Map([["totalBudgetTeam", 0]]);

  const result = team.reduce((accum, spec) => {
    const costEach = Math.trunc(
      salaries[spec.specialization].salary /
        (1 - salaries[spec.specialization].tax.replace("%", "") / 100)
    );

    if (accum.has(`totalBudget${spec.specialization}`)) {
      accum.set(
        `totalBudget${spec.specialization}`,
        accum.get(`totalBudget${spec.specialization}`) + costEach
      );

      accum.set("totalBudgetTeam", accum.get("totalBudgetTeam") + costEach);

      return accum;
    }

    accum.set(`totalBudget${spec.specialization}`, costEach);

    accum.set("totalBudgetTeam", accum.get("totalBudgetTeam") + costEach);

    return accum;
  }, accum);
  return Object.fromEntries(result);
}

const salaries1 = {
  Manager: { salary: 1000, tax: "10%" },
  Designer: { salary: 600, tax: "30%" },
  Artist: { salary: 1500, tax: "15%" },
};

const team1 = [
  { name: "Misha", specialization: "Manager" },
  { name: "Max", specialization: "Designer" },
  { name: "Vova", specialization: "Designer" },
  { name: "Leo", specialization: "Artist" },
];

const financeReport1 = calculateTeamFinanceReport(salaries1, team1);

console.log(financeReport1);

const salaries2 = {
  TeamLead: { salary: 1000, tax: "99%" },
  Architect: { salary: 9000, tax: "34%" },
};

const team2 = [
  { name: "Alexander", specialization: "TeamLead" },
  { name: "Gaudi", specialization: "Architect" },
  { name: "Koolhas", specialization: "Architect" },
  { name: "Foster", specialization: "Architect" },
];

const financeReport2 = calculateTeamFinanceReport(salaries2, team2);

console.log(financeReport2);

# IESA-Opt-EUP
**IESA-Opt-EUPS (European power system model)**
## Running the model
You need some pre-requistics before running the model:
- The IESA-Opt-EUP model is written in AIMMS. Thus, you need to install this windows-based application to be able to run the model. You can obtain your free academic licence from this link: https://licensing.cloud.aimms.com/license/academic.htm
- Although you can choose any mathematical solver for the solving procedure, the IESA-Opt-EUPS model runs best with the Gurobi mathematical solver. You can obtain your free academic licence from this link: https://www.gurobi.com/academia/academic-program-and-licenses/

Please feel free to contact us if you face issues. 

## Model description

### From IESA-Opt to IESA-Opt-EUPS

We use the IESA-Opt model implemented for the EU + Switzerland, Norway and the UK to capture European wide effects. This is a detailed open-source optimization for the power sector on country level. IESA-Opt models investments of the energy system over the horizon from 2020 to 2050 in 5-year time steps while simultaneously accounting for hourly and daily operational constraints. The model's objective function minimizes the net present value of energy system costs to achieve total energy needs under certain techno-economic and policy constraints (e.g., a specific greenhouse gas (GHG) reduction target in a particular year). It is an open-source and flexible model that can be used for other regions or countries (e.g., the Netherlands Energy system, or the North Sea region energy system).

Exogenous parameters: Crossborder tranmission capacities are fix in the base cases, but this can also be optimised in LP capacity expansion also. Vintage capacity is inlcuded for 2020 and it can be expanded towards 2050. Resource availbaility is inluded for bioenergy, solar, wind (last two are hourly), hourly demand patterns and deteailed technoeconomic assumptions are all included exogenously.

Options include retrofitting and negative emission technology in the form of BECCS. Also in the indirect emission version, all GHG emissions can be included, as well as external CO2 removal option.

The IESA-Opt model reflects the emission constraints of the EU Emission Trading System (ETS), the non-ETS sectors, and the international navigation and aviation sectors. Since ETS sector emissions are traded in the EU ETS market, we assume an exogenous ETS emission price projection as a scenario parameter. Because the national emission reduction policy targets both ETS and non-ETS sectors, we set the aggregate national emission constraint on both sectors. If the constraint is binding, the model generates an aggregated national emission shadow price, equal to the marginal increase in the system cost if the aggregated emission constraint gets one unit tighter. 

**Objective function definition**

The IESA-Opt model’s objective function refers to the system’s net present value resulting from the set of decision variables confirmed by annualized investments, decommissioning, retrofitting, and use of technologies. However, this objective function does not account for the cost of technology stock in periods after the investment period. While, the fixed operational and variable operation costs related to the invested technology continue to affect the objective function in the subsequent periods. In this formulation, the weight of investment decisions in earlier periods is undervalued. Therefore, the system tends to make more significant investments in earlier periods as it does not pay for the annualized capital cost in successive periods. Although this formulation minimizes the net present value of investment decisions, it does not minimize the system costs of the energy system. 

Therefore, we modify the objective function by adding the investment matrix before the capital component to represent total system costs. The binary investment matrix determines the presence of a technology option in each period based on its economic lifetime. 

Moreover, we add a social discount factor (SDF) to account for the net present value of costs at each period. This discount factor is based on the assumed exogenous social discount rate that describes how society values future investments. The social discount rate should not be confused with the capital depreciation rate. The capital depreciation rate or Weighted Average Cost of Capital (WACC) is used to annualize the overnight capital investment costs. Although WACC can be different for each technology, we assume a 5% rate for all technologies in the reference scenario. Thus, with the addition of the social discount rate, the new objective function calculates the sum of the net present value of energy transition costs:

![image](https://user-images.githubusercontent.com/63007753/162712451-14900ad1-9d37-48d5-b552-e02a8066fda9.png)

With this objective function formulation, the capital cost of technologies is accounted for during their economic lifetime. However, the investments in the last modeling period may be distorted as the benefits of investments after this period are neglected. Since 2050 is crucial in current policies, we do not want these so-called end-of-horizon effects in 2050. Although this effect is already reduced by using annualized investment costs, we add two more periods (i.e., 2055 and 2060) to the model’s horizon to further reduce this effect. Since the additional periods aim to represent investment costs better, all energy system definitions, including activity levels and technological costs and potentials, are kept equal to their value in 2050. 

** New GHG emission constraint **

In this version we have adapted the emission constraint, in a way that capacity installation related emissions can also be included (e.g. manufacturing, constraction etc).
